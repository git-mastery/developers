---
title: Hands-on structure
parent: Exercises
nav_order: 7
---

All hands-ons are stored within the `hands_on` folder of the [`exercises`](https://github.com/git-mastery/exercises) repository.

They are represented by a single Python file whose name is the hands-on ID.

Hands-ons are not graded and progress is not tracked. They only help set up the student's folder structure for a given lesson.

{: .note }

Git-Mastery uses the format `hp-<hands-on-name>` for hands-on names, for example `hp-init-repo`, to differentiate them from exercise names. Internally, the `hands_on/` folder stores the Python files, for example `hands_on/init_repo.py`.

## File structure

Given below is a simple example:

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

The setup instructions go under the `download` function.

`__requires_git__` and `__requires_github__` tell the Git-Mastery app whether to run automatic verification that the student has set up Git and GitHub CLI correctly.
