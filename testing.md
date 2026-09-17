# Testing Standards

## Scripted playtest: required, blocking

Every game repo has a scripted smoke playtest that runs the **real game**
(not a mock) and asserts the core loop. It is a required status check —
a failing playtest blocks the merge button, full stop.

## How it runs

- `tests/` holds the harness (excluded from game exports, never shipped to players).
- CI runs the game under `xvfb` with software GL (Mesa llvmpipe):
  `xvfb-run -a godot --path . res://tests/smoke_test.tscn --rendering-driver opengl3`
- The test drives the game with synthetic input (`Input.action_press(...)`),
  captures a screenshot per stage, and asserts outcomes.
- Exit code 0 = pass, 1 = fail. Any `SCRIPT ERROR` in the log also fails the run.
- Screenshots go to `test-results/screenshots/` (gitignored) and are uploaded
  as CI artifacts, always — even on failure, for debugging.

## Writing robust tests

- **Frame-counted waits**, never wall-clock sleeps.
- **Tolerant thresholds**, never exact values: "moved more than 2m", "jumped more
  than 1m". Physics timing jitters in CI; assertions must not.
- Assert observable outcomes (label text, visibility, positions), not internals.
- Cover the loop that matters: spawn → move → jump → core mechanic (collect, etc.)
  → win/lose state → respawn/restart.

## What automation catches vs. what it doesn't

- Catches: crashes on load, frozen input, broken jump, uncollectable items,
  dead triggers, script errors — the "refactor broke everything silently" class.
- Doesn't catch: feel. Floaty jumps, unfair platforms, bad camera — that's
  Isaac's department as feel QA. He playtests every meaningful change.

## CI hardening (learned 2026-09-16)

The first CI run timed out at the 15-minute job limit with zero output — the
playtest step redirected everything to a file, so the "hang" was invisible.
Fixes, now standard:

- **Stream logs live**: pipe the game output through `tee` so the step shows
  progress in real time; keep the file for the tail/grep checks.
- **Print stage markers** in the test (`SMOKE: stage ...`) so silence is never
  ambiguous — you can see exactly where a slow run is.
- **Shrink the viewport**: `--resolution 640x360` cuts software-rendered pixels
  4x. llvmpipe on a 2-vCPU runner is slow; screenshots stay legible.
- **Fail loudly on hangs**: `timeout -k 60 25m` on the step plus a generous
  `timeout-minutes` on the job. A timeout is a clear failure, not a mystery.
- **Fixed frame counts can't hang the test itself** — every wait is a bounded
  number of physics frames. If a run stalls, suspect rendering/import speed,
  not the wait logic. (Unbounded `while` waits are still forbidden.)

## Video capture

The playtest records the virtual display (ffmpeg x11grab) during key moments
and uploads the clip as a CI artifact, linked from the PR. Screenshots show
states; video shows motion.
