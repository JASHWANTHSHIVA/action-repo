# action-repo

This is the **trigger repository** for the TechStaX GitHub Webhook Integration project.

## Purpose

This repo is a dummy repository whose sole purpose is to trigger GitHub Actions on:
- **Push** events (any branch)
- **Pull Request** events (opened, reopened, synchronized)
- **Merge** events (bonus — when a PR is merged)

All events are forwarded as structured JSON payloads to the `webhook-repo` Flask endpoint.

## How It Works

| Event | GitHub Trigger | Action Sent |
|-------|---------------|-------------|
| Push to any branch | `push` | `PUSH` |
| PR opened/updated | `pull_request` (opened, reopened, synchronize) | `PULL_REQUEST` |
| PR merged | `pull_request` (closed + merged=true) | `MERGE` |

## Setup

### 1. Add GitHub Secret
Go to **Settings → Secrets and variables → Actions** and add:

| Secret Name | Value |
|-------------|-------|
| `WEBHOOK_URL` | Your deployed `webhook-repo` Flask app URL (e.g., `https://your-app.ngrok.io`) |

### 2. Workflows
- `.github/workflows/push_webhook.yml` — Handles Push events
- `.github/workflows/pr_merge_webhook.yml` — Handles PR and Merge events

### 3. Triggering Events
- **Push**: Just push any commit to any branch
- **Pull Request**: Open a PR from a feature branch to main
- **Merge**: Merge the PR on GitHub

## Webhook Payload Format

```json
{
  "request_id": "<commit_sha or PR-<number>>",
  "author": "<github_username>",
  "action": "PUSH | PULL_REQUEST | MERGE",
  "from_branch": "<source_branch>",
  "to_branch": "<target_branch>",
  "timestamp": "<DD Month YYYY - HH:MM AM/PM UTC>"
}
```

## Related Repository
- **webhook-repo**: The Flask app that receives, stores (MongoDB), and displays events via UI.
