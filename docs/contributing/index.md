---
title: Contributing
nav_order: 2
---

[![CI](https://github.com/git-mastery/exercises/actions/workflows/ci.yml/badge.svg)](https://github.com/git-mastery/exercises/actions/workflows/ci.yml)

Both hands-on and exercises reside in the same repository: [`git-mastery/exercises`](https://github.com/git-mastery/exercises). So before you start to contributing, you should have these setup already:

- Bash environment
- Python 3.13
- Github CLI [installed and authenticated](https://github.com/cli/cli#installation) for testing the download script

## Setup

1. Fork the repository: <https://github.com/git-mastery/exercises>
2. Clone the fork

    ```bash
    git clone https://github.com/<username>/exercises
    ```

3. Setup a virtual environment

    ```bash
    python -m venv venv
    ```

4. Activate the virtual environment

    ```bash
    source venv/bin/activate
    ```

    If you are using Windows, you should run the following instead: 

    ```bash
    source venv/Scripts/activate
    ```

5. Install all dependencies

    ```bash
    pip install -r requirements.txt
    ```
