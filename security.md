# Security: secret handling for public repos

Game repos are public by default only once they earn it — but even private
repos get the full treatment, because anything uploaded can become public
later. Secrets are handled with a **triple-check**, and "triple" is literal:
three independent layers, and an upload needs all three green.

## The triple-check (mandatory before ANY GitHub upload)

1. **Human review.** I read the exact file list and diff of everything being
   uploaded. Nothing goes up sight-unseen.
2. **Local scanner.** `scripts/scan-secrets.py` (stdlib-only Python) runs
   against the full tree; exit 0 is required. Three sub-layers:
   - sensitive filenames (`.env`, `*.pem`, `*credential*`, private keys...),
   - ~15 secret patterns (AWS keys, GitHub/Slack/Stripe tokens, private key
     headers, `api_key`/`password` assignments, credentialed URLs, ...),
   - high-entropy strings that look like generated secrets.
   Positive-controlled: it catches planted AWS keys, passwords, private keys,
   and tokens.
3. **CI gitleaks.** `.github/workflows/secrets.yml` runs
   `gitleaks/gitleaks-action@v3` on every PR and push as a **required status
   check**. Even if layers 1 and 2 somehow miss something, the merge is
   blocked. (Note: use `@v3`, not `@v2` — v2's Node 20 runtime was removed
   from GitHub runners on 2026-09-16.)

If a secret is ever found in an upload: rotate it immediately, purge it from
git history, treat it as an incident, not a cleanup.

## Repo lockdown for not-ready games

Until a game is ready for outside eyes:

- Issues, Wiki, and Projects tabs disabled (`has_issues/wik/projects: false`).
- No outside collaborators — owner merges everything.
- Branch protection already means nobody but the owner can merge, so a
  drive-by PR can never land code; it can only sit unmerged.
- Honest limitation: on a **public** repo GitHub does not let you prevent
  forks from *opening* PRs. If zero inbound PRs is required, the repo must
  go private (which also disables Pages on free accounts). Middle ground
  (adopted): an auto-close bot workflow (`.github/workflows/autoclose.yml`)
  that immediately closes PRs from anyone other than the owner/Isaac with a
  "not accepting contributions yet" note. It uses `pull_request_target` so it
  runs from the base branch even for fork PRs, and checks out no code, so
  untrusted PR contents can never execute. Note: it only takes effect after
  the workflow itself is merged to `main`.

## Naming lesson (2026-09-16)

Don't name a workflow `secrets.yml` — the scanner's filename denylist flags
it. `secret-scan.yml` scans clean.

## Repo visibility

Private by default; public only when the game earns it. Never let "I want a
quick Pages preview link" quietly make a rough prototype public — Pages on
free accounts requires public, so treat going public and enabling Pages as
one deliberate decision.
