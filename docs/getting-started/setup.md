---
title: Development setup
parent: Getting started
nav_order: 1
---

# Development setup

Git-Mastery is standardizing on a `uv`-first local development workflow.

Use the guidance on this page as the default when setting up repositories locally. Some repositories are still in transition, and may be using `pip` and `venv` for local development.

## Prerequisites

- Git
- Python 3.13+
- `uv` (refer to [installation guide](https://docs.astral.sh/uv/getting-started/installation/))
- GitHub CLI (`gh`) installed and authenticated when working on flows that interact with GitHub

## Repository setup (uv)

1. Fork the repository you want to work on.
2. Clone your fork.

   ```bash
   git clone https://github.com/<username>/<repository>
   cd <repository>
   ```

3. Run the following command to set up virtual environment and install dependencies:

   ```bash
   uv sync
   ```

4. Set up pre-commit hooks using LeftHook

   {: .note }

   > Note
   >
   > Recommended to install for formatting and linting support.

   ```bash
   uv run lefthook install
   ```

## Repository setup (pip)

{: .warning-title }

> Deprecated
>
> As we are transitioning to uv, this setup is still relevant for some repositories.

1. Fork the repository you want to work on.
2. Clone your fork.

   ```bash
   git clone https://github.com/<username>/<repository>
   cd <repository>
   ```

3. Run the following command to set up virtual environment and install dependencies:

   ```bash
   pip install -r requirements.txt
   python -m venv .venv
   source .venv/bin/activate # macOS/Linux
   source .venv/Scripts/activate # Windows
   ```

4. Set up pre-commit hooks using LeftHook

   {: .note }

   > Note
   >
   > Recommended to install for formatting and linting support.

   ```bash
   lefthook install
   ```

## GitHub-dependent work

If you are working on downloads, remote exercises, or progress sync flows, ensure `gh auth status` succeeds before testing.

Git-Mastery's GitHub-dependent commands also expect the `delete_repo` scope for flows that create and remove forks.

```bash
gh auth refresh -s delete_repo
```
