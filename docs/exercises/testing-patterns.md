---
title: Testing guide
parent: Exercises
nav_order: 6
---

# Testing guide

Exercise verification tests usually focus on repository end states rather than replaying the entire student workflow.

## Recommended approach

1. Use `GitAutograderTestLoader` from `exercise_utils.test`
2. Build the repository state you need with `repo-smith`
3. Run `verify(...)`
4. Assert on `GitAutograderStatus` and important comments

## Basic pattern

```python
from git_autograder import GitAutograderStatus
from exercise_utils.test import GitAutograderTestLoader, assert_output

from .verify import verify

REPOSITORY_NAME = "my-exercise"

loader = GitAutograderTestLoader(REPOSITORY_NAME, verify)


def test_base() -> None:
    with loader.start() as (test, rs):
        rs.git.create_file("notes.txt", "hello")
        rs.git.add(["notes.txt"])
        rs.git.commit("Add notes")

        output = test.run()

        assert_output(output, GitAutograderStatus.SUCCESSFUL)
```

## What the loader gives you

`GitAutograderTestLoader` sets up a temporary exercise directory, patches exercise config loading, and constructs a real `GitAutograderExercise` object for the test run.

Within `with loader.start() as (test, rs):`

- `test.run()` executes the exercise's `verify(...)`
- `rs` is a [`RepoSmith`](/developers/docs/libraries/repo-smith) helper rooted at the student's working repository. It abstracts the underlying Git repository, letting you set up state with calls like `rs.git.commit(...)`, `rs.git.create_file(...)`, etc.

## Useful test options

### Start from an existing repository

Use `clone_from=...` when the exercise needs a prepared repository as its starting point.

```python
with loader.start(clone_from="https://github.com/git-mastery/some-repo") as (test, rs):
    ...
```

### Mock `answers.txt`

Use `mock_answers` for answer-driven exercises.

```python
with loader.start(mock_answers={"What is Git?": "Version control"}) as (test, rs):
    output = test.run()
```

### Include a remote repository

Use `include_remote_repo=True` when verification depends on remote behavior (eg. pushing to remote).

```python
with loader.start(include_remote_repo=True) as (test, rs, rs_remote):
    ...
```

## What to assert

- `output.status` for the overall result
- key comments, not necessarily every comment
- repository state only when it helps explain the verification behavior

`exercise_utils.test.assert_output(...)` is useful when you want to check the status and ensure specific comments are present.

## Testing guidance

- Prefer one test per verification rule or scenario
- Keep each test focused on the minimal repository state needed
- Store verification comments as constants in `verify.py` so tests can import them safely
- Mock remote interactions instead of making real network calls

## Running tests

In `exercises`, run:

```bash
./test.sh <exercise-folder>
```

Or run the full suite with:

```bash
pytest . -s -vv
```
