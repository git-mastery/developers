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
- `exercise_utils.exercise_config`: helpers for updating `.gitmastery-exercise.json`, including adding PR metadata
- `exercise_utils.file`: file creation and appending helpers
- `exercise_utils.git`: common Git operations such as commit, repository initialization, and adding files
- `exercise_utils.gitmastery`: Git-Mastery specific functions such as creating the start tag
- `exercise_utils.github_cli`: GitHub CLI wrappers for repository operations and pull request workflows such as creating, viewing, commenting on, reviewing, merging, and closing PRs
- `exercise_utils.roles`: role-marker helpers for generating teammate-authored commits, merges, pull requests, comments, reviews, and PR closures
- `exercise_utils.test`: testing utilities including `GitAutograderTestLoader` and `assert_output` for unit testing verification scripts

### `exercise_utils.roles`

`RoleMarker(role)` is useful when an exercise needs to simulate teammate-authored Git or GitHub activity while still using the normal helper APIs.

- It automatically prefixes commit messages, merge messages, PR titles and bodies, PR comments, and PR reviews with a `[ROLE:...]` marker
- It also provides utilities for formatting, detecting, extracting, and stripping role markers when you need to work with the text directly

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
