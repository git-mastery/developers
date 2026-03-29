---
title: App
nav_order: 5
has_children: true
---

# App

The [`app`](https://github.com/git-mastery/app) repository contains the `gitmastery` CLI.

## Key Features

- Set up a local Git-Mastery workspace
- Fetch hands-ons and exercises from the configured exercises source
- Execute verification logic from the `exercises` repository
- Maintain local and remote progress tracking

## Command Reference

Refer to [command reference](https://git-mastery.org/companion-app/index.html#git-mastery-app-commands) for the full list of commands, their descriptions, and usage examples.

{: .warning-title }

> Note
>
> Experimental command `gitmastery` to spawn a REPL instance with CLI runtime loaded. Not all commands may work as expected in the REPL, and behavior may differ from running the CLI directly. <br/> <br/>
> The REPL accepts both Git-Mastery commands and shell commands:
>
> - Use `/verify` or `gitmastery verify` for Git-Mastery commands
> - Use `cd` to change directories inside the REPL
> - Any other command is executed through the local shell

## Testing expectations

`app` has an end-to-end test suite under `app/tests/e2e` that exercises the built CLI across setup, check, download, verify, progress, version, and REPL flows.

When you add a new command or change command behavior, update or add E2E tests for the user-visible behavior. Command-level unit tests are useful, but they do not replace the cross-platform CLI coverage provided by the E2E suite.

Typical local flow:

```bash
uv run pyinstaller --onefile main.py --name gitmastery
export GITMASTERY_BINARY="$PWD/dist/gitmastery"
uv run pytest tests/e2e/ -v
```
