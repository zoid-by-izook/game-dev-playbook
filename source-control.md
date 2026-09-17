# Source Control Strategy

GitHub Flow, tightened for a small team where the agent writes most code.

## The loop

1. Feature branch off `main` (`feat/...`, `fix/...`, `test/...`). Messy commits welcome here.
2. Open a PR into `main`.
3. CI runs: build + scripted playtest (see `testing.md`).
4. Both green → **squash-merge**. Exactly one commit lands on `main` per PR.
5. Push to `main` auto-deploys (e.g. GitHub Pages).

## Why squash

- `main` stays perfectly linear: one commit = one PR = one tested, working change.
- `git bisect` works because every `main` commit passed CI.
- No "wip" / "oops" noise in permanent history.
- The PR's full commit history stays visible on GitHub, linked from the squash commit.

## Branch protection on `main`

- Pull request required (no direct pushes — applies to the agent too).
- Required status checks: build **and** playtest. Strict mode (branch must be up to date).
- Required linear history (rejects merge commits; belt and suspenders with squash).
- Force pushes and branch deletion blocked.
- Zero required human reviewers — CI is the gate. Isaac can still review anything before merge.

## Commit messages: Conventional Commits

Squash-merge titles use prefixes: `feat:`, `fix:`, `test:`, `chore:`, `docs:`.
Example: `feat: add start menu (#4)`.

Why: `git log --oneline` reads like a summary, `git log --grep=fix` narrows blame
fast, and tooling can generate changelogs from the prefixes.

## Pushing from the agent environment

The stored GitHub credential's surrogate exchange only works for `api.github.com`,
and GitHub rejects password auth for git operations — so plain `git push` over
HTTPS does not work here. Push via the git database API instead
(create blobs → tree → commit → update ref), or the Contents API for single files.
Remote history may therefore differ slightly from the local clone; that's expected.
