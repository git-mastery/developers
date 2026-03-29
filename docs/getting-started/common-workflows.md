---
title: Common workflows
parent: Getting Started
nav_order: 2
---

# Common workflows

## Choosing a repository

- Use `exercises` for new hands-ons, exercises, and verification tests.
- Use `app` for CLI behavior, download flows, and local progress features.
- Use `repo-smith` when exercise verification tests need new repository setup primitives.
- Use `git-autograder` when verification logic needs new reusable grading helpers.

## Common local tasks

- Install dependencies:
  - `app`, `repo-smith`, `git-autograder`: `uv sync`
  - `exercises`: `uv venv && source .venv/bin/activate && uv pip install -r requirements.txt`
- Run Python tests:
  - `app`: `uv run pytest`
  - `repo-smith`: `uv run pytest -s -vv`
  - `git-autograder`: `uv run pytest -s -vv`
  - `exercises`: `./test.sh <exercise-folder>` or `pytest . -s -vv`
- Run the CLI locally from `app`: `uv run python main.py`
- Test exercise downloads from `exercises`: `./test-download.sh <name>`

## Common contribution paths

### Author or update an exercise

1. Work in `exercises`
2. Implement `download.py`, `verify.py`, and `test_verify.py`
3. Test the download flow locally
4. Test the verification flow locally

### Change CLI behavior

1. Work in `app`
2. Update the relevant command or config logic
3. Re-run the local CLI using `uv run python main.py`
4. Test against a local Git-Mastery exercises directory

### Extend verification helpers

1. Work in `git-autograder` when the helper should be reusable across exercises
2. Work in `repo-smith` when the missing piece is repository-state setup for tests

## Recommended contributor path

If you are new to Git-Mastery, start by contributing to `exercises`, then move to `app` or the supporting libraries once you are familiar with the exercise lifecycle.
