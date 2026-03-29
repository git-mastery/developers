---
title: App
nav_order: 5
has_children: true
---

# App

The [`app`](https://github.com/git-mastery/app) repository contains the `gitmastery` CLI.

## What this section covers

- CLI responsibilities in the wider Git-Mastery ecosystem
- The main command groups and flows
- Configuration files and local developer behavior
- How the app interacts with exercise verification and progress tracking

## Main responsibilities

- Set up a local Git-Mastery workspace
- Fetch hands-ons and exercises from the configured exercises source
- Execute verification logic from the `exercises` repository
- Maintain local and remote progress tracking

## Suggested reading

1. [CLI command reference](/developers/docs/app/command-reference)
2. [Configuration reference](/developers/docs/app/configuration)
3. [Download and verification flow](/developers/docs/app/download-and-verify-flow)
4. [Progress tracking](/developers/docs/app/progress-integration)

## Testing expectations

`app` has an end-to-end test suite under `app/tests/e2e` that exercises the built CLI across setup, check, download, verify, progress, version, and REPL flows.

When you add a new command or change command behavior, update or add E2E tests for the user-visible behavior. Command-level unit tests are useful, but they do not replace the cross-platform CLI coverage provided by the E2E suite.

Typical local flow:

```bash
uv run pyinstaller gitmastery.spec
export GITMASTERY_BINARY="$PWD/dist/gitmastery"
uv run pytest tests/e2e/ -v
```
