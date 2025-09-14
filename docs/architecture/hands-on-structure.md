---
title: Hands-on Structure
parent: Architecture
nav_order: 1
---

All hands-ons are stored within the `hands_on` folder of the [`exercises`](https://github.com/git-mastery/exercises).

They are represented by a single Python file, whose name is the hands-on ID (replacing `_` with `-`).

Hands-ons are not graded and the progress is not tracked. They are only present to help setup the student's folder structure for a given lesson!

{: .note }

Git-Mastery uses the format `hp-<hands-on-name>` for hands-on names (e.g., `hp-init-repo`), to differentiate them from exercise names. Internally, we use a `hands_on/` folder (instead of the `hp-` prefix) to differentiate hands-on files from exercise files (e.g., `hands_on/init_repo.py`).

## File structure

Since the hands-on is only comprised of a single file, there isn't a lot of complexity to it. Given below is an example:

```python
import os

from exercise_utils.cli import run_command
from exercise_utils.file import append_to_file, create_or_update_file
from exercise_utils.git import add, init

__requires_git__ = True
__requires_github__ = False


def download(verbose: bool):
    os.makedirs("things")
    os.chdir("things")
    init(verbose)
    create_or_update_file(
        "fruits.txt",
        """
        apples
        bananas
        cherries
        """,
    )
    add(["fruits.txt"], verbose)
    run_command(["git", "add", "fruits.txt"], verbose)
    append_to_file("fruits.txt", "dragon fruits")

```

The setup instructions of the hands-on go under the `download` function.

`__requires_git__` and `__requires_github__` tells the Git-Mastery app whether to run automatic verification that the student has already setup Git and/or Github CLI correctly!
