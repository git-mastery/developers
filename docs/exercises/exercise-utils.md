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

## Contributing utility functions

These should cover most use cases, but if there is no existing utility function for your use case, `exercise_utils.cli.run_command` is available to execute other CLI calls.

If you believe that more utility functions should be supported, feel free to open a pull request adding one.

{: .note }

If you add a new file such as `exercise_utils/general.py`, remember to update `app` to include the file in the `EXERCISE_UTILS_FILES` constant.
