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

- `schedule`: once daily at `10:00 AM EAT` (`07:00 UTC`)
- `workflow_dispatch`: manual run from the Actions tab

To run manually:

1. Open the **Actions** tab.
2. Select **Accept EaglepointAiLabs Invites**.
3. Click **Run workflow**.

## How to confirm it worked

After a run starts, open the workflow run and check both the step logs and the run summary.

### 1) Step logs (source of truth)

In the **Accept matching repository invitations** step, look for:

- Per-invitation success lines: `Accepted invitation <id> for <owner/repo>`
- Per-invitation failure lines: `Failed to accept invitation <id> for <owner/repo>: <error>`
- Final aggregate line: `Aggregate results: Total=<n>, Matches=<n>, Accepted=<n>, Failed=<n>`

A clean successful run for matching invites will typically end with `Failed=0`.

### 2) Run summary (quick view)

The workflow also writes a **run summary** with:

- totals (`Total invitations`, `Matching invitations`, `Accepted invitations`, `Failed invitations`)
- an **Accepted invitations** list
- a **Failed invitations** list (only shown when failures exist)

This gives a fast, human-readable confirmation without scanning all raw logs.

### 3) Step outputs (structured values)

The workflow sets these outputs on the invitation step:

- `total_invitations`
- `matching_invitations`
- `accepted_invitations`
- `failed_invitations`

These values are echoed in the next step so they are easy to inspect in the logs and reusable by future workflow steps.

## Security notes for a public automation repo

- Repository secrets are encrypted and not exposed to normal workflow logs.
- Do **not** accept pull requests that modify workflow files without careful review.
- Keep workflow permissions minimal (this workflow sets `contents: read`).
- Use a least-privilege PAT and rotate it periodically.
