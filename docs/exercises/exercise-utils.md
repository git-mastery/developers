---
title: Exercise utilities reference
parent: Exercises
nav_order: 7
---

# Exercise utilities reference

These utilities are contained within the [`exercises/exercise_utils` module](https://github.com/git-mastery/exercises/tree/main/exercise_utils).

They are loaded during exercise download, which means that the `app` has access to these functions while setting up a hands-on or exercise.

## Available utility modules

- `exercise_utils.cli`: generic CLI calls
- `exercise_utils.file`: file creation and appending helpers
- `exercise_utils.git`: common Git operations such as commit, repository initialization, and adding files
- `exercise_utils.gitmastery`: Git-Mastery specific functions such as creating the start tag
- `exercise_utils.github_cli`: GitHub CLI wrappers for forking, cloning, deleting, and creating repositories, as well as looking up the authenticated user
- `exercise_utils.test`: testing utilities including `GitAutograderTestLoader` and `assert_output` for unit testing verification scripts

### `exercise_utils.cli` functions

The existing utility functions should cover most use cases, but if there is no existing utility function for your use case, `exercise_utils.cli` functions are available to execute other CLI calls. These should be used as a last resort.

`exercise_utils.cli` exposes three functions for running shell commands:

- `run_command`: runs a command and exits the process if it fails; use for steps that must succeed
- `run_command_no_exit`: runs a command and returns `None` on failure without exiting; use for optional or conditional steps
- `run`: runs a command and returns a `CommandResult` you can inspect (`.is_success()`, `.stdout`, `.returncode`); use when you need to branch on the result

## Contributing utility functions

If you believe that more utility functions should be supported, feel free to open a pull request adding one.

{: .note }

If you add a new file such as `exercise_utils/general.py`, remember to update `app` to include the file in the `EXERCISE_UTILS_FILES` constant.
