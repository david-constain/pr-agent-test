# PR-Agent test (exact copy of gotrhythm/rhythm-pulse setup)

Repro of `.github/workflows/pr_agent.yml` + `.pr_agent.toml` from `rhythm-pulse`,
to isolate org policy vs config failures.

## Setup

1. Repo secret: `ANTHROPIC_API_KEY`
2. Actions enabled for this repo

## How to trigger

Only slash commands on a PR (same as rhythm-pulse — `pull_request` auto-run is commented out):

```text
/review
/describe
```

## What we learned from the first test

The Action exited green but posted only "Preparing review…" because Anthropic returned:

```text
not_found_error: model: claude-3-5-sonnet-20241022
```

(and the same for `claude-3-5-haiku-20241022`). That is very likely why rhythm-pulse also "stopped working" — retired model IDs, not org policy.

This repo keeps the rhythm-pulse workflow/toml structure, but uses current Anthropic model IDs:

- `anthropic/claude-sonnet-4-5-20250929`
- fallback: `anthropic/claude-haiku-4-5-20251001`
