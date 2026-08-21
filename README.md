# PR-Agent test

Minimal personal-repo repro for [Qodo PR-Agent](https://github.com/qodo-ai/pr-agent) with Anthropic.

Used to isolate whether failures in `gotrhythm/rhythm-pulse` are org policy vs config/version.

## One-time setup

1. Add a repository secret:
   - GitHub → **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `ANTHROPIC_API_KEY`
   - Value: your Anthropic API key

2. Ensure Actions are enabled for this repo:
   - **Settings → Actions → General → Allow all actions**

## How to test

### Auto-run on PR open

```bash
git checkout -b test/pr-agent
echo "hello from pr-agent" >> test_file.txt
git add test_file.txt
git commit -m "test: trigger pr-agent"
git push -u origin HEAD
gh pr create --fill
```

Open the PR and wait for the **PR-Agent** workflow. You should get describe + review comments.

### Slash commands

On the PR, comment:

```text
/review
```

or:

```text
/describe
```

A new workflow run should start for that comment.

## What success / failure means

| Result | Likely conclusion for rhythm-pulse |
|---|---|
| Works here | Org policy, secrets, or Action allowlist on `gotrhythm` |
| Fails here too (auth) | Anthropic key / model config |
| No workflow runs | Actions disabled or trigger `if:` not matching |

## Differences from rhythm-pulse setup

- Uses Docker image `pragent/pr-agent:0.34.2-github_action` (Anthropic auth fix)
- `pull_request` trigger enabled (auto-run)
- Slash commands still work via `issue_comment`
- Clean `[config]` toml (not the older `[general]` layout)
