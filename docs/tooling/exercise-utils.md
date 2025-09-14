---
title: Exercise utilities
nav_order: 1
parent: Tooling
---

These are all contained within the [`exercises/exercise_utils` module](https://github.com/git-mastery/exercises/tree/main/exercise_utils).

These are loaded when downloading an exercise, which means that the `app` has access to these functions.

## Available utility functions

There are functions broadly for the following categories:

- `exercise_utils.cli`: Generic CLI calls
- `exercise_utils.file`: File creation/appending
- `exercise_utils.git`: Common Git operations like commit, initializing a repository, and adding files
- `exercise_utils.gitmastery`: Git-Mastery specific functions like creating the start tag

## Contributing utility functions

These should cover around 80% of all use cases, but if there isn't an existing utility function for your use case, `exercise_utils.cli.run_command` is available to execute any other CLI calls.

If you believe that more utility functions should be supported, feel free to open a PR adding one.

{: .note }

If you add a new file, for example `exercise_utils/general.py`, please update `app` to include the file in the `EXERICSE_UTILS_FILES` constant [here](https://github.com/git-mastery/app/blob/6786aa998e9c8f7ca63c77534bfaf5b7514a89c2/app/utils/gitmastery.py#L212).
