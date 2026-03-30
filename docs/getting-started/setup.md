---
title: Development setup
parent: Getting started
nav_order: 1
---

# Development setup

Git-Mastery is standardizing on a `uv`-first local development workflow.

Use the guidance on this page as the default when setting up repositories locally.

{: .warning }

> Some repositories are still in transition, and may be using `pip` and `venv` for local development.

## Prerequisites

- Git
- Python 3.13+
- `uv` (refer to [installation guide](https://docs.astral.sh/uv/getting-started/installation/))
- GitHub CLI (`gh`) installed and authenticated when working on flows that interact with GitHub
- Git-Mastery app installed and configured (refer to [the setup guide](https://git-mastery.org/companion-app/index.html#installation-and-setup))

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

   > Recommended to install for formatting and linting support.

   ```bash
   lefthook install
   ```
