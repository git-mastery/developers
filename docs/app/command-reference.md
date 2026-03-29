---
title: CLI command reference
parent: App
nav_order: 1
---

# CLI command reference

The `gitmastery` CLI is the main entrypoint for local Git-Mastery workflows.

If the CLI is launched without a subcommand, it starts an interactive REPL instead of exiting immediately.

## Main command areas

- `setup`: initialize local Git-Mastery configuration
- `check`: verify Git and GitHub CLI prerequisites
- `download`: download a hands-on or exercise
- `verify`: verify the current exercise attempt
- `progress`: inspect, reset, or sync learner progress
- `version`: print the current app version

## Command groups

### `setup`

- Creates a new Git-Mastery root directory
- Writes `.gitmastery.json`
- Creates a local `progress/` folder with `progress.json`

### `check`

- `gitmastery check git`: checks Git installation, minimum version, `user.name`, and `user.email`
- `gitmastery check github`: checks GitHub CLI installation, authentication, and `delete_repo` scope

These checks are also invoked automatically by other commands when required by an exercise or workflow.

### `download`

- Downloads either a hands-on or an exercise from the configured exercises source
- Hands-ons are identified by the `hp-` prefix
- Exercises use `.gitmastery-exercise.json` plus `download.py` to set up the student workspace

### `verify`

- Loads the current exercise's metadata
- Executes the exercise's `verify.py`
- Prints the grading result
- Records progress locally and optionally syncs it remotely

### `progress`

- `gitmastery progress show`: show the latest known status for each exercise
- `gitmastery progress reset`: reset the current exercise workspace and remove its saved progress entries
- `gitmastery progress sync on`: connect the local tracker to a GitHub fork of the progress repository
- `gitmastery progress sync off`: remove remote sync and keep a local-only progress tracker

## REPL behavior

The REPL accepts both Git-Mastery commands and shell commands:

- Use `/verify` or `gitmastery verify` for Git-Mastery commands
- Use `cd` to change directories inside the REPL
- Any other command is executed through the local shell

## E2E test coverage

The current E2E suite lives under `app/tests/e2e` and includes coverage for:

- `check`
- `setup`
- `download`
- `verify`
- `progress`
- `version`
- the REPL

If you add a new top-level command or change the visible behavior of an existing command, add or update E2E tests in `app/tests/e2e` so the built CLI behavior stays covered.

The CI workflows run the E2E suite through `uv run pytest tests/e2e/ -v` after building the binary and setting `GITMASTERY_BINARY`.
