# Game Development Playbook

Isaac's standing practices and standards for building games on GitHub.
Applies to every game project unless he overrides for a specific one.

## Files

- `source-control.md` — branching, PRs, squash merges, branch protection, commit style
- `pr-standards.md` — how PR descriptions are written (Why / Strategy / Verification)
- `testing.md` — scripted playtests as a required merge gate, screenshots, video
- `releases.md` — changelog, versioning, GitHub Releases
- `godot-setup.md` — Godot project conventions (versions, exports, CI)

## Core principles

1. **Main is append-only.** Linear history, one tested commit per change, never rewritten.
2. **CI is the gate.** A PR merges only when automated checks pass — build *and* playtest.
3. **PRs respect the reader.** Why and strategy up front, details in the code.
4. **Every PR shows the game.** Screenshots always; video of key moments.
5. **Isaac is the feel QA.** Automated tests catch breakage; he judges feel, fairness, vibe.
