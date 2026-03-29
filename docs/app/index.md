---
title: App
nav_order: 5
has_children: true
has_toc: false
---

# App

The [`app`](https://github.com/git-mastery/app) repository contains the `gitmastery` CLI.

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
5. [E2E testing flow](/developers/docs/app/e2e-testing-flow)
6. [Publishing flow](/developers/docs/app/publishing-flow)
7. [How to add a command](/developers/docs/app/how-to-add-a-command)

## Testing expectations

`app` has an end-to-end test suite under `app/tests/e2e` that exercises the built CLI across setup, check, download, verify, progress, version, and REPL flows.

When you add a new command or change command behavior, update or add E2E tests for the user-visible behavior. See [E2E testing flow](/developers/docs/app/e2e-testing-flow) for a full guide.
