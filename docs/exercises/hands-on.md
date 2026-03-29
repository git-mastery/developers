---
title: How to add a hands-on
parent: Exercises
nav_order: 1
---

# How to add a hands-on

## What is a hands-on?

[Lessons accompanying Git-Mastery App](https://nus-cs2103-ay2526s1.github.io/website/se-book-adapted/git-trail/index.html) contains hands-on practicals for students to get hands-on experience of the Git concepts covered by the lesson. Some of those hands-on practicals need to set up a sandbox containing the required folders, files, and repositories before the student can start. Git-Mastery app can set up that sandbox for students so that they can get to the practical part more easily.

For example, a student can run `gitmastery download hp-init-repo` to set up a sandbox for a specific hands-on practical.

Some examples of how Git-Mastery app helps create the sandbox for a hands-on practical can be seen in [this Git Tour](https://nus-cs2103-ay2526s1.github.io/website/book/gitAndGitHub/trail/recordingFolderHistory/index.html) (see T1L3 and T1L4).

## Before contributing

New contributors are recommended to start by implementing an [already approved hands-on proposal](https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22hands-on%20discussion%22%20label%3A%22help%20wanted%22).

If you are proposing a new hands-on instead, make sure that you have done the following:

- [ ] Create a [hands-on discussion](https://github.com/git-mastery/exercises/issues/new?template=hands_on_discussion.yaml)
- [ ] Obtain approval on the hands-on
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Implementing a hands-on

First, run the provided `new.sh` script to generate the scaffolding for a new hands-on sandbox:

```bash
./new.sh
```

The script will first prompt if you want to create a hands-on or exercise.

Enter `hands-on` or `h` to create a new hands-on.

Then, the script will prompt you for:

1. The name of the hands-on, likely specified in the corresponding hands-on discussion, without the `hp-` prefix. Use kebab case.
2. Whether the hands-on requires Git
3. Whether the hands-on requires GitHub

Each hands-on is implemented as a single file at `hands_on/<hands-on name>.py`, containing the instructions to set up the hands-on sandbox.

## Hands-on file format

All hands-ons are stored within the `hands_on` folder of the [`exercises`](https://github.com/git-mastery/exercises) repository.

They are represented by a single Python file whose name is the hands-on ID.

Hands-ons are not graded and progress is not tracked. They only help set up the student's folder structure for a given lesson.

{: .note }

Git-Mastery uses the format `hp-<hands-on-name>` for hands-on names, for example `hp-init-repo`, to differentiate them from exercise names. Internally, the `hands_on/` folder stores the Python files, for example `hands_on/init_repo.py`.

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
    append_to_file("fruits.txt", "dragon fruits")
```

The setup instructions go under the `download` function.

`__requires_git__` and `__requires_github__` tell the app whether to run automatic verification that the student has set up Git and GitHub CLI correctly.

{: .reference }

For more information about how Git-Mastery downloads a hands-on, refer to the [Download flow](/developers/docs/exercises/download-workflow).

{: .note }

> `exercises` comes with a set of utility functions in the `exercise_utils` module that are made available during the download flow. Prefer these abstractions over raw CLI calls — for example, use `exercise_utils.file.create_or_update_file` to create or update files, and `exercise_utils.git.commit` to commit changes. Fall back to `exercise_utils.cli.run_command` only when no suitable abstraction exists.
>
> For the full list of utility functions, refer to [Exercise utilities reference](/developers/docs/exercises/exercise-utils).

These are some references for download setups for other hands-ons:

- [init-repo](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/init_repo.py)
- [add-files](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/add_files.py)
- [stage-modified](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/stage_modified.py)

### Conventions

1. Any operations should use OS-agnostic options, for example `shutil.rmtree` instead of `run_command(["rm"])`.
2. For any commands that require `gh`, make sure that the `__requires_github__` variable in the download setup is set to `True` so that the app automatically checks that the student has set up GitHub CLI correctly.

### Testing downloads

To test that your download script works, use the provided script:

```bash
./test-download.sh <hands-on name>
```

You can find the sandbox created by the script under `test-downloads/`. Check it manually to verify that it is as expected.

## Submitting the hands-on for review

Create a pull request from your fork using the provided pull request template.

## Example

1. Hands-on discussion: <https://github.com/git-mastery/exercises/issues/44>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Hands-on PR: <https://github.com/git-mastery/exercises/pull/45>
