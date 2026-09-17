# PR Standards

Every PR description follows the template in `.github/pull_request_template.md`:

## Why
The problem or opportunity, in a sentence or two.

## Strategy
The approach taken and the key decisions behind it. A reader should understand
the plan without opening the code. Deliberately rejected alternatives belong
here, briefly.

## Verification
How it was tested: CI checks, playtest results, screenshots/video.

## The bar

- **Accurate and succinct.** A reader learns *why* the PR exists and *what strategy*
  it took, without being burdened by details they can get from the code.
- No restating the diff. No filler. If a section needs more than a short paragraph,
  the change is probably too big — split the PR.
- The template lives in the repo so the standard is visible at PR creation time.

## Visual evidence (habit)

- Every PR links gameplay **screenshots** in Verification (uploaded as CI artifacts
  from the playtest — never committed to the repo).
- **Video** of key gameplay moments, captured automatically during the playtest
  (ffmpeg x11grab of the virtual display, uploaded as a CI artifact).
- For UI-heavy changes (menus, HUD), grab dedicated captures beyond the playtest shots.
