# GitHub Invite Auto-Accept (eaglepointAiLabs only)

This repository contains a GitHub Actions workflow that automatically accepts pending **private repository collaboration invitations** only when either:

- the inviter is `eaglepointAiLabs`, or
- the repository owner is `eaglepointAiLabs`.

All other invitations are ignored.

## Setup

1. Create a Personal Access Token (PAT) for the account that receives invitations.
   - Use a fine-grained token or classic token with the minimum permissions required to read and accept repository invitations.
2. In this automation repository, go to **Settings → Secrets and variables → Actions → New repository secret**.
3. Create a secret named:

   - `INVITE_ACCEPT_TOKEN`

4. Paste the PAT value and save.

## How it runs

Workflow file: `.github/workflows/accept-eaglepoint-invites.yml`

Triggers:

- `schedule`: every 10 minutes
- `workflow_dispatch`: manual run from the Actions tab

To run manually:

1. Open the **Actions** tab.
2. Select **Accept EaglepointAiLabs Invites**.
3. Click **Run workflow**.

## How to confirm it worked

After each run, open the workflow run in the **Actions** tab and check:

- Step logs from **Accept matching repository invitations** for lines like:
  - `Accepted invitation <id> for <owner/repo>`
  - `Invitation processing complete. Total=<n>, Matches=<n>, Accepted=<n>, Failed=<n>`
- The run summary section **Invitation Acceptance Summary**, which includes totals and accepted/failed invite details.

Interpretation:

- `Accepted > 0` and `Failed = 0`: matching invitations were accepted successfully.
- `Matches = 0`: nothing eligible to accept (expected/idempotent).
- `Failed > 0`: at least one invitation could not be accepted; check warning lines for the specific invitation and reason.

## Security notes for a public automation repo

- Repository secrets are encrypted and not exposed to normal workflow logs.
- Do **not** accept pull requests that modify workflow files without careful review.
- Keep workflow permissions minimal (this workflow sets `contents: read`).
- Use a least-privilege PAT and rotate it periodically.
