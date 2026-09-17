# Art & Credits Standards

How third-party art, audio, and fonts enter a game — and how the artists
behind them get credited. Established 2026-09-17 during the test-game art
upgrade.

## Credits scaffolding comes first

Before the first third-party asset lands, every surface that will credit it
must exist:

1. **In-game credits menu** — reachable from the title/start menu, listing one
   entry per asset: title, artist, license. Build it data-driven (e.g. a
   `CREDITS` array the menu renders at runtime) so asset PRs only append
   entries.
2. **Website credits section** — on the game's GitHub Pages site.
3. **README credits section** — in the repo README.

Scaffolding is infra: it can merge without art review.

## Credit as you go

Credits are updated **in the same PR** as the asset they cover — in-game,
website, and README, all three, every time. Never "we'll credit it later."
A PR that adds an asset without its credits is incomplete.

## Research per item, not all at once

Don't research every asset category upfront. Work one item at a time:

1. Research 2–3 options for the item (public, cleanly licensed — CC0
   preferred so attribution edge cases never arise).
2. Present options with previews/screenshots to Isaac.
3. Isaac picks. Only then implement.
4. The PR holds for his feel/critique and merges only on his say-so.

## Merge rule: art needs a human

- **Art-based PRs** (anything involving art direction — models, textures,
  skyboxes, shaders chosen for look, fonts, sounds): **no auto-merge.**
  Present asset options and in-game screenshots/video, wait for Isaac's
  feel/critique, merge only on his explicit say-so.
- **Infra, code, or screenshot-verifiable PRs**: may auto-merge once CI is
  green (Isaac still gets the PR link on every merge, per the standing
  workflow).

## Licensing

- Prefer **CC0** sources (e.g. Kenney.nl) — no attribution requirements, no
  edge cases. Credit them anyway; it's the right thing to do.
- Record the license per asset in all three credit surfaces.
- Never commit an asset whose license you haven't read.
