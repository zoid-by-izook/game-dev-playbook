# Releases & Changelog

## Don't hand-write the changelog — generate it

- **release-drafter**: on every merge to `main`, drafts GitHub Release notes
  grouped by PR labels (Features / Fixes / Chores). Zero local tooling;
  notes live next to the versioned releases.
- If an in-repo changelog is ever wanted, **git-cliff** can generate
  `CHANGELOG.md` from Conventional Commits (Keep a Changelog format).
  Start with release-drafter; add `CHANGELOG.md` only if asked.

## Versioning

- `VERSION` file at repo root, SemVer.
- Bump per release, tag `v0.2.0`, publish the GitHub Release.
- Tags make bug-blame precise: "broke somewhere between v0.2.0 and v0.3.0"
  beats "broke sometime last month".

## What we're skipping

GitFlow (develop branch, release branches) — that's for scheduled releases with
QA gates. These projects are continuous-deploy: every merge ships.
