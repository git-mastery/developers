---
title: How to add an exercise
parent: Exercises
nav_order: 2
---

# How to add an exercise

1. TOC
{:toc}

## Before contributing

If you are proposing a new exercise, instead of implementing an [already approved exercise proposal](https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22exercise%20discussion%22%20label%3A%22help%20wanted%22), make sure that you have done the following:

- [ ] Create an [exercise discussion](https://github.com/git-mastery/exercises/issues/new?template=exercise_discussion.yaml)
- [ ] Obtain approval on the exercise
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Create a new exercise

Use the provided `new.sh` script to generate the scaffolding for a new exercise:

```bash
./new.sh
```

The script will first prompt if you want to create a hands-on or exercise.

Enter `exercise` or `e` to create a new exercise.

Then, the script will prompt you for:

1. The name of the exercise, likely specified in the corresponding exercise discussion. Use kebab case.
2. The exercise tags, split by spaces, likely specified in the corresponding exercise discussion.
3. The exercise repository type. The scaffold currently supports `local`, `remote`, and `ignore` directly.

{: .reference }

Refer to the [Exercise format reference](/developers/docs/exercises/exercise-structure) page for more information about the generated folder structure and repository type options.

## Download setup

`download.py` contains the instructions to set up the local repository.

The app loads `download.py` dynamically from the exercises source, reads `__resources__` if present, and executes `setup(...)` inside the student's working repository.

{: .reference }

For more information about how Git-Mastery downloads exercises, refer to the [Download flow](/developers/docs/exercises/download-workflow).

{: .note }

> `exercises` comes with a set of utility functions in the `exercise_utils` module that are made available during the download flow. Prefer these abstractions over raw CLI calls — for example, use `exercise_utils.file.create_or_update_file` to create or update files, and `exercise_utils.git.commit` to commit changes. Fall back to `exercise_utils.cli.run_command` only when no suitable abstraction exists.
>
> For the full list of utility functions, refer to [Exercise utilities reference](/developers/docs/exercises/exercise-utils).

These are some references for download setups for other exercises:

- [ignoring-somethings](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/ignoring_somethings/download.py)
- [branch-bender](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/branch_bender/download.py)
- [amateur-detective](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/amateur_detective/download.py)

### Download conventions

1. Any operations should use OS-agnostic options, for example `shutil.rmtree` instead of `run_command(["rm"])`.
2. If you need to compare repository state before and after the student's work, create the start tag during setup using the shared Git-Mastery utilities.
3. For any commands that require `gh`, make sure that the `requires_github` configuration is set to `true` so that the app automatically checks that the student has set up GitHub CLI correctly.
4. `setup(...)` may receive a `repo-smith` helper object as `rs`; use it instead of shelling out directly when possible.

### Testing downloads

To test that your download script works, use the provided script:

```bash
./test-download.sh <your exercise folder>
```

You can find the downloaded repository under `test-downloads/`.

## Verification setup

The verification process is controlled by `verify.py`. This file contains the grading logic that inspects the student's work and returns a result with a status and feedback comments.

{: .reference }

For more information about how Git-Mastery verifies exercise attempts, refer to the [Verification flow](/developers/docs/exercises/verification-workflow).

The [`git-autograder`](https://github.com/git-mastery/git-autograder) library builds the `GitAutograderExercise` object passed to `verify(...)`. It exposes the exercise config, the working repository, and optional answer parsing.

If there is no helper for a check you need, you can still fall back to the underlying `GitPython` repository:

```python
from git_autograder import (
    GitAutograderExercise,
    GitAutograderOutput,
    GitAutograderStatus,
)


def verify(exercise: GitAutograderExercise) -> GitAutograderOutput:
    # Access the underlying GitPython repo:
    exercise.repo.repo

    return exercise.to_output([], GitAutograderStatus.SUCCESSFUL)
```

Refer to existing `verify.py` scripts to understand the available helper functions that streamline grading. Open an issue if there is something that is not yet supported or if you have a question.

For `repo_type: ignore` exercises, `exercise.repo` is a null repository wrapper. In that mode, verify against files such as `answers.txt` or exercise metadata rather than Git state.

Some examples of verifications:

- [branch-bender](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/branch_bender/verify.py)
- [conflict-mediator](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/conflict_mediator/verify.py)

### Verification conventions

1. Store the comments of the verification as constants so that they can be imported and used reliably in unit tests.
2. For any remote behavior to verify, provide a mock to substitute the behavior in unit tests.
3. Prefer raising `exercise.wrong_answer([...])` for incorrect submissions and reserve unexpected exceptions for genuine errors.

### Testing verify logic

Use [`repo-smith`](https://github.com/git-mastery/repo-smith) to simulate possible student answers and verify that your grading logic correctly accepts valid attempts and flags expected mistakes. 

Refer to existing `test_verify.py` files to see examples of unit testing the verification script.

{: .reference }

See [Testing guide](/developers/docs/exercises/testing-patterns) for the recommended `GitAutograderTestLoader` workflow.

You can run the unit tests of your exercise via:

```bash
./test.sh <your exercise folder>
```

## Submitting the exercise for review

Create a pull request from your fork using the provided pull request template.

## Example

1. Exercise discussion: <https://github.com/git-mastery/exercises/issues/24>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Exercise PR: <https://github.com/git-mastery/exercises/pull/26>
