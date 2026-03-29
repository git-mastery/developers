---
title: Setup
parent: Getting Started
nav_order: 1
---

# Setup

Git-Mastery is standardizing on a `uv`-first local development workflow.

Use the guidance on this page as the default when setting up repositories locally. Some repositories are still in transition, so the exact `uv` command differs slightly by repository.

## Prerequisites

- Git
- Python 3.13+
- [`uv`](https://docs.astral.sh/uv/getting-started/installation/)
- GitHub CLI (`gh`) installed and authenticated when working on flows that interact with GitHub

## Recommended repository setup

1. Fork the repository you want to work on.
2. Clone your fork.
3. Set up the environment with the repository's `uv` workflow.

```bash
git clone https://github.com/<username>/<repository>
cd <repository>
```

## Repository-specific setup

### `app`

`app` already has `pyproject.toml` and `uv.lock`, so the standard workflow is:

```bash
uv sync
uv run lefthook install
uv run python main.py
```

### `repo-smith`

`repo-smith` is already a Python package, so the standard workflow is:

```bash
uv sync
uv run pytest -s -vv
```

### `git-autograder`

`git-autograder` also has package metadata, so the standard workflow is:

```bash
uv sync
uv run pytest -s -vv
```

### `exercises`

`exercises` has not fully migrated to a `uv sync`-based project layout yet, but you can still use `uv` as the package manager for local development:

```bash
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

The repository still contains older helper scripts such as `setup.sh`, `test.sh`, and `test-download.sh`. They remain useful, but new documentation should prefer `uv` where practical.

## GitHub-dependent work

If you are working on downloads, remote exercises, or progress sync flows, ensure `gh auth status` succeeds before testing.

Git-Mastery's GitHub-dependent commands also expect the `delete_repo` scope for flows that create and remove forks.

```bash
gh auth refresh -s delete_repo
```
