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

## Security notes for a public automation repo

- Repository secrets are encrypted and not exposed to normal workflow logs.
- Do **not** accept pull requests that modify workflow files without careful review.
- Keep workflow permissions minimal (this workflow sets `contents: read`).
- Use a least-privilege PAT and rotate it periodically.
