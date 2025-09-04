---
title: Hands-on Structure
parent: Architecture
nav_order: 1
---

All hands-ons are stored within the `hands_on` folder of the [`exercises`](https://github.com/git-mastery/exercises).

They are represented by a single Python file, whose name is the hands-on ID (replacing `_` with `-`).

Hands-ons are not graded and the progress is not tracked. They are only present to help setup the student's folder structure for a given lesson!

{: .note }

Git-Mastery users will prefix `hp-` before the hands-on names, but internally, we do not use this prefix as the `hands_on` folder is sufficient for us.

## File structure

Since the hands-on is only comprised of a single file, there isn't a lot of complexity to it.

```python
import subprocess
from sys import exit
from typing import List, Optional

__requires_git__ = True
__requires_github__ = True


def run_command(command: List[str], verbose: bool) -> Optional[str]:
    try:
        result = subprocess.run(
            command,
            capture_output=True,
            text=True,
            check=True,
        )
        if verbose:
            print(result.stdout)
        return result.stdout
    except subprocess.CalledProcessError as e:
        if verbose:
            print(e.stderr)
        exit(1)


def download(verbose: bool):
    pass
```

The setup instructions of the hands-on go under `download`.

`__requires_git__` and `__requires_github__` tells the Git-Mastery app whether to run automatic verification that the student has already setup Git and/or Github CLI correctly!
