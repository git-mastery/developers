---
title: git-autograder reference
parent: Shared libraries
nav_order: 2
---

# git-autograder reference

`git-autograder` is the grading library that loads a Git-Mastery exercise attempt and turns repository or answer checks into structured verification results.

## Installation

```bash
pip install git-autograder
```

## Basic structure of a `verify.py`

```python
from git_autograder import (
    GitAutograderExercise,
    GitAutograderOutput,
    GitAutograderStatus,
)

SOME_ERROR = "You haven't done X yet."


def verify(exercise: GitAutograderExercise) -> GitAutograderOutput:
    main = exercise.repo.branches.branch("main")

    if not main.user_commits:
        raise exercise.wrong_answer([SOME_ERROR])

    return exercise.to_output(["Well done!"], GitAutograderStatus.SUCCESSFUL)
```

The app calls `verify(exercise)` directly. All exception handling and output formatting is done by the app.

---

## `GitAutograderExercise`

The main object passed to every `verify(exercise)` function.

| Attribute / Method | Description |
|---|---|
| `exercise.exercise_name` | Exercise identifier (from config) |
| `exercise.exercise_path` | Path to the exercise root directory |
| `exercise.config` | Parsed `.gitmastery-exercise.json` as `ExerciseConfig` |
| `exercise.repo` | `GitAutograderRepo` (or `NullGitAutograderRepo` for `ignore` exercises) |
| `exercise.answers` | Parsed `answers.txt` as `GitAutograderAnswers` |
| `exercise.to_output(comments, status)` | Build the return value |
| `exercise.wrong_answer(comments)` | Raise a grading failure |
| `exercise.read_config(key)` | Read a key from `.gitmastery-exercise.json` |
| `exercise.write_config(key, value)` | Write a key to `.gitmastery-exercise.json` |

### Exception types

| Exception | When to use |
|---|---|
| `GitAutograderWrongAnswerException` | The student's attempt is incorrect → `UNSUCCESSFUL` |
| `GitAutograderInvalidStateException` | The exercise is in an invalid state → `ERROR` |

Use `raise exercise.wrong_answer([...])` for grading failures. Reserve bare exceptions for unexpected errors only.

---

## `exercise.repo` — repository helpers

`exercise.repo` is a `GitAutograderRepo` for most exercises. For `ignore` and `local-ignore` exercises it is a `NullGitAutograderRepo` that raises on any Git access.

### `exercise.repo.branches` — `BranchHelper`

```python
branch = exercise.repo.branches.branch("main")       # raises if missing
branch = exercise.repo.branches.branch_or_none("main")  # returns None if missing
exists = exercise.repo.branches.has_branch("feature/login")
```

#### `GitAutograderBranch`

| Property / Method | Description |
|---|---|
| `branch.name` | Branch name |
| `branch.commits` | All commits (newest first) |
| `branch.user_commits` | Commits after the start tag (student's work) |
| `branch.latest_commit` | Most recent commit |
| `branch.latest_user_commit` | Most recent student commit |
| `branch.start_commit` | The Git-Mastery start tag commit |
| `branch.reflog` | List of `GitAutograderReflogEntry` |
| `branch.has_non_empty_commits()` | True if any student commit changed files |
| `branch.has_edited_file(path)` | True if the file was modified since the start tag |
| `branch.has_added_file(path)` | True if the file was added since the start tag |
| `branch.checkout()` | Checkout this branch |

Example — check that the student committed on `main`:

```python
main = exercise.repo.branches.branch("main")
if not main.user_commits:
    raise exercise.wrong_answer(["You have no commits on main yet."])
```

### `exercise.repo.commits` — `CommitHelper`

```python
commit = exercise.repo.commits.commit("HEAD")
commit = exercise.repo.commits.commit_or_none("abc1234")
```

#### `GitAutograderCommit`

| Property / Method | Description |
|---|---|
| `commit.hexsha` | Full commit SHA |
| `commit.stats` | GitPython `Stats` object (files changed) |
| `commit.parents` | List of `GitAutograderCommit` |
| `commit.branches` | Branch names that contain this commit |
| `commit.is_child(parent)` | True if this commit descends from `parent` |
| `commit.file_change_type(file)` | `"A"`, `"M"`, `"D"`, or `None` |
| `commit.file(path)` | Context manager yielding the file contents at this commit |
| `commit.checkout()` | Detach HEAD to this commit |

### `exercise.repo.remotes` — `RemoteHelper`

```python
remote = exercise.repo.remotes.remote("origin")          # raises if missing
remote = exercise.repo.remotes.remote_or_none("origin")  # returns None
exists = exercise.repo.remotes.has_remote("origin")
```

#### `GitAutograderRemote`

Wraps a GitPython `Remote`.

| Method | Description |
|---|---|
| `remote.remote` | The underlying GitPython `Remote` object |
| `remote.is_for_repo(owner, repo_name)` | True if the remote URL points to `owner/repo_name` on GitHub (supports both HTTPS and SSH URLs) |
| `remote.track_branches(branches)` | Check out remote-tracking branches locally |

Example — verify the remote points to the right repository:

```python
origin = exercise.repo.remotes.remote("origin")
if not origin.is_for_repo("git-mastery", "exercises"):
    raise exercise.wrong_answer(["Your remote 'origin' does not point to the correct repository."])
```

### `exercise.repo.tags` — `TagHelper`

```python
tag = exercise.repo.tags.tag("v1.0.0")                 # raises if missing
tag = exercise.repo.tags.tag_or_none("v1.0.0")         # returns None if missing
exists = exercise.repo.tags.has_tag("v1.0.0")

remote_tags = exercise.repo.tags.remote_tag_names("origin")          # raises if remote is missing or tags cannot be queried
remote_tags = exercise.repo.tags.remote_tag_names_or_none("origin")  # returns None on missing remote or query failure
```

#### `GitAutograderTag`

| Property / Method | Description |
|---|---|
| `tag.name` | Tag name |
| `tag.commit` | Commit pointed to by this tag (`GitAutograderCommit`) |
| `tag.is_annotated` | True if this is an annotated tag |
| `tag.is_lightweight` | True if this is a lightweight tag |
| `tag.message_or_none(strip=True, lower=False)` | Annotated tag message, or `None` for lightweight tags |
| `tag.points_to(commit)` | True if this tag points to the given commit |

Example — check that a required local tag exists:

```python
if not exercise.repo.tags.has_tag("v1.0.0"):
    raise exercise.wrong_answer(["Tag 'v1.0.0' is missing."])
```

Example — verify that the start tag exists on `origin`:

```python
main = exercise.repo.branches.branch("main")
first_commit = main.commits[-1]
start_tag = f"git-mastery-start-{first_commit.hexsha[:7]}"

remote_tags = exercise.repo.tags.remote_tag_names_or_none("origin")
if remote_tags is None or start_tag not in remote_tags:
    raise exercise.wrong_answer([f"Missing start tag on origin: {start_tag}"])
```

### `exercise.repo.files` — `FileHelper`

```python
# Open a file if it exists
with exercise.repo.files.file_or_none("notes.txt") as f:
    content = f.read() if f else None

# Open a file (raises if missing)
with exercise.repo.files.file("notes.txt") as f:
    content = f.read()

# List untracked files
untracked = exercise.repo.files.untracked_files()
```

### Raw GitPython access

If no helper covers your use case, access the underlying GitPython repo directly:

```python
exercise.repo.repo   # GitPython Repo object
```

---

## `exercise.answers` — answer file support

For exercises where students fill in an `answers.txt` file.

### `answers.txt` format

```
Q: What does git add do?
A: Stages changes for the next commit

Q: What does git commit do?
A: Records staged changes to the repository
```

### Usage

`answers.question(q)` and `answers.question_or_none(q)` return a `GitAutograderAnswersRecord` with two attributes:

| Attribute | Description |
|---|---|
| `record.question` | The question string |
| `record.answer` | The student's answer string |

```python
answers = exercise.answers

# Get a specific answer (raises GitAutograderInvalidStateException if missing)
record = answers.question("What does git add do?")
print(record.answer)

# Safe access — returns None if the question is not present
record = answers.question_or_none("What does git add do?")
if record is None:
    raise exercise.wrong_answer(["Missing answer for 'What does git add do?'"])

# Validate answers with rules, then call validate() to apply them all at once
from git_autograder.answers.rules.not_empty_rule import NotEmptyRule
from git_autograder.answers.rules.has_exact_value_rule import HasExactValueRule
from git_autograder.answers.rules.contains_value_rule import ContainsValueRule

answers.add_validation("What does git add do?", NotEmptyRule())
answers.add_validation("Name a git command", HasExactValueRule("git commit"))
answers.validate()  # raises GitAutograderWrongAnswerException if any rule fails
```

### Available answer rules

| Rule | Description |
|---|---|
| `NotEmptyRule` | Answer must not be blank |
| `HasExactValueRule(value)` | Answer must equal `value` exactly (case-insensitive) |
| `ContainsValueRule(value)` | Answer must contain `value` (case-insensitive) |
| `ContainsListRule(values)` | Answer must contain all values in the list |
| `HasExactListRule(values)` | Answer must match all values in the list exactly |

All rules are in `git_autograder.answers.rules`.

### In tests

Mock `answers.txt` content via `GitAutograderTestLoader`:

```python
with loader.start(mock_answers={"What does git add do?": "Stages changes"}) as (test, rs):
    output = test.run()
    assert_output(output, GitAutograderStatus.SUCCESSFUL)
```

---

## `GitAutograderStatus`

| Value | Meaning |
|---|---|
| `SUCCESSFUL` | Student completed the exercise correctly |
| `UNSUCCESSFUL` | Student's attempt is incorrect or incomplete |
| `ERROR` | Exercise is in an invalid or unexpected state |

---

## `GitAutograderOutput`

Built by `exercise.to_output(comments, status)`. Contains:

| Field | Type | Description |
|---|---|---|
| `exercise_name` | `str` | Exercise identifier |
| `started_at` | `datetime` | When `GitAutograderExercise` was constructed |
| `completed_at` | `datetime` | When `to_output` was called |
| `comments` | `List[str]` | Feedback shown to the student |
| `status` | `GitAutograderStatus` | Final result |

---

## Full example — branch check

```python
from git_autograder import (
    GitAutograderExercise,
    GitAutograderOutput,
    GitAutograderStatus,
)

NOT_ON_MAIN = "You aren't on the main branch. Run 'git checkout main'."
NO_COMMITS = "You haven't committed your changes yet."
SUCCESS = "Great work! Your changes are committed to main."


def verify(exercise: GitAutograderExercise) -> GitAutograderOutput:
    try:
        active = exercise.repo.repo.active_branch.name
    except TypeError:
        raise exercise.wrong_answer(["You are in a detached HEAD state."])

    if active != "main":
        raise exercise.wrong_answer([NOT_ON_MAIN])

    main = exercise.repo.branches.branch("main")
    if not main.user_commits:
        raise exercise.wrong_answer([NO_COMMITS])

    return exercise.to_output([SUCCESS], GitAutograderStatus.SUCCESSFUL)
```

{: .reference }

See [How to add an exercise](/developers/docs/exercises/exercise) and [Verification flow](/developers/docs/exercises/verification-workflow) for how this fits into the full exercise lifecycle.
