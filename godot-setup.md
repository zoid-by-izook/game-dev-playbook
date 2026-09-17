# Godot Project Conventions

## Versions

- Pin the Godot version per repo (e.g. `4.7.2`) in CI env and document it.
  The binary and export templates must match exactly.
- First verified: Godot 4.7.2 stable, `4.7.2.stable.official`.

## Project layout

- `scenes/`, `scripts/`, `icon.svg`, `project.godot`, `export_presets.cfg`,
  `README.md`, `VERSION` at root.
- `tests/` — playtest harness, excluded from exports via the preset's
  `exclude_filter` (`tests/*`).
- `test-results/` — local playtest output (screenshots), gitignored.
- `.godot/` (import cache) and `exports/` (local build output) — gitignored,
  CI rebuilds both fresh.

## CI: two workflows

- **Build** (`deploy.yml`): downloads the pinned Godot binary + matching export
  templates, runs the headless Web export, uploads the Pages artifact, deploys
  to GitHub Pages on `main`. Uses current official actions
  (`actions/checkout`, `actions/upload-pages-artifact`, `actions/deploy-pages`)
  with `pages: write` / `id-token: write` permissions.
- **Playtest** (`playtest.yml`): scripted smoke test under xvfb (see `testing.md`).
  Both are required status checks on `main`.

## Repo ownership (standing rule)

- Default: new game repos go under the machine user `zoid-by-izook`,
  never Isaac's personal `Izook` account (his is a carefully curated list).
- Invite `Izook` as a collaborator so he can access from his account
  (he must accept the invite).
- Public repos when GitHub Pages is needed (Pages requires public on free accounts).
