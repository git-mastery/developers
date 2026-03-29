---
title: git-autograder reference
parent: Shared libraries
nav_order: 2
---

# git-autograder reference

`git-autograder` is the verification library used by Git-Mastery exercise `verify.py` scripts.

## Typical use in Git-Mastery

- Load and inspect a student's exercise repository
- Produce structured grading comments and statuses
- Provide reusable helpers over raw Git access

## Basic example

```python
from git_autograder import (
    GitAutograderExercise,
    GitAutograderOutput,
    GitAutograderStatus,
)


def verify(exercise: GitAutograderExercise) -> GitAutograderOutput:
    return exercise.to_output([], GitAutograderStatus.SUCCESSFUL)
```

## Core concepts

### `GitAutograderExercise`

This is the main entrypoint used by exercise verification scripts.

It:

- reads `.gitmastery-exercise.json`
- loads the student's working repository
- exposes `exercise_name`, `config`, and `repo`
- provides `to_output(...)` and `wrong_answer(...)`

### `GitAutograderOutput`

Verification scripts return a `GitAutograderOutput` containing:

- `status`
- `started_at`
- `completed_at`
- `comments`
- `exercise_name`

### `GitAutograderStatus`

The current statuses are:

- `SUCCESSFUL`
- `UNSUCCESSFUL`
- `ERROR`

## Repository modes

For normal exercises, `exercise.repo` wraps a real Git repository.

For `repo_type: ignore`, `exercise.repo` becomes a null repository wrapper. Accessing Git-specific properties on it raises an error, which helps catch verification code that assumes a repository exists when it should not.

## Answers support

`GitAutograderExercise.answers` parses `answers.txt` from the exercise root on demand.

It supports question-and-answer style validation and can accumulate validation rules before calling `validate()`.

## Typical verification flow

1. Construct `GitAutograderExercise`
2. Inspect repository state through `exercise.repo`
3. Optionally inspect `exercise.answers`
4. Raise `exercise.wrong_answer([...])` for incorrect submissions
5. Return `exercise.to_output([...], GitAutograderStatus.SUCCESSFUL)` on success

{: .reference }

See [How to add an exercise](/developers/docs/exercises/exercise) and [Verification flow](/developers/docs/exercises/verification-workflow) for how `git-autograder` is used inside the main Git-Mastery exercise flow.

For implementation details, refer to the [`git-autograder` repository](https://github.com/git-mastery/git-autograder).
