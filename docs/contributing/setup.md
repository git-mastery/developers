---
title: Setup
parent: Contributing
nav_order: 1
---

Both hands-on and exercises reside in the same repository: [`git-mastery/exercises`](https://github.com/git-mastery/exercises). So before you start to contributing, you should have these setup already:

- Bash environment
- Python 3.13+
- Github CLI [installed and authenticated](https://github.com/cli/cli#installation) for testing the download script
- uv [installed](https://docs.astral.sh/uv/getting-started/installation/)

## Setup

1. Fork the repository: <https://github.com/git-mastery/exercises>
2. Clone the fork

   ```bash
   git clone https://github.com/<username>/exercises
   ```

3. Run the following command to set up virtual environment and install dependencies:

   ```bash
   uv sync
   ```

4. Set up pre-commit hooks (for linting, formatting and type checking) using LeftHook

   ```bash
   uv run lefthook install
   ```
