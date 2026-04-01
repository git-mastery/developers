---
title: repo-smith reference
parent: Shared libraries
nav_order: 1
---

# repo-smith reference

`repo-smith` is a test-support library that creates and mutates local and remote Git repository states through helper objects, so exercise verification scenarios can be set up deterministically in tests.

{: .warning }

The YAML-based spec format from repo-smith v1 is deprecated. All Git-Mastery exercises use the Python v2 API directly through `create_repo_smith` and the `RepoSmith` helper objects.

## Installation

```bash
pip install -U repo-smith
```

## Core API

### `create_repo_smith`

The main entrypoint. Returns a context manager that yields a `RepoSmith` instance.

```python
from repo_smith.repo_smith import create_repo_smith

with create_repo_smith(verbose=False) as rs:
    rs.git.commit(message="Initial commit", allow_empty=True)
```

#### Options

| Option | Type | Description |
|---|---|---|
| `verbose` | `bool` | Print commands as they run |
| `existing_path` | `str` | Use an existing directory instead of creating a temp one |
| `clone_from` | `str` | Clone from a URL before starting |
| `null_repo` | `bool` | Create a `RepoSmith` with no underlying repository |

### `RepoSmith`

Yielded by `create_repo_smith`. Provides three built-in helpers:

| Attribute | Description |
|---|---|
| `rs.git` | Git operations |
| `rs.files` | File system operations |
| `rs.gh` | GitHub CLI operations |
| `rs.repo` | Underlying `GitPython` `Repo` object |

Custom helpers can be registered with `rs.add_helper(MyHelper)` and accessed with `rs.helper(MyHelper)`.

---

## `rs.git` — Git operations

### Commit and stage

```python
rs.git.add("notes.txt")
rs.git.add(["a.txt", "b.txt"])
rs.git.add(all=True)

rs.git.commit(message="Initial commit", allow_empty=True)
rs.git.commit(message="Add file")
```

### Branches

```python
rs.git.checkout("feature/login", branch=True)   # create and switch
rs.git.checkout("main")                           # switch to existing branch
rs.git.branch("feature/payments")                 # create without switching
rs.git.branch("new-name", old_branch="old-name", move=True)  # rename
rs.git.branch("old-branch", delete=True)          # delete
```

### Merge

```python
rs.git.merge("feature/login", no_ff=True)
rs.git.merge("feature/dashboard")
```

### Tags

```python
rs.git.tag("v1.0")
rs.git.tag("v1.0", "abc1234")  # tag a specific commit
```

### Reset and revert

```python
rs.git.reset("HEAD~1", hard=True)
rs.git.revert("HEAD")
```

### Remotes and push/fetch

```python
rs.git.remote_add("origin", "https://github.com/example/repo.git")
rs.git.remote_rename("origin", "upstream")
rs.git.remote_remove("origin")

rs.git.push("origin", "main", set_upstream=True)
rs.git.push("origin", ":old-branch")   # the colon prefix deletes the remote branch
rs.git.fetch("origin")
rs.git.fetch(all=True)
```

### Restore

```python
rs.git.restore("notes.txt")
rs.git.restore("notes.txt", staged=True)
```

---

## `rs.files` — File operations

```python
rs.files.create_or_update("notes.txt", "hello world")  # create or overwrite
rs.files.create_or_update("empty.txt")                  # create empty file
rs.files.append("notes.txt", "\nmore content")
rs.files.delete("notes.txt")
rs.files.delete("some-dir/")
rs.files.mkdir("subdir")
rs.files.cd("subdir")
```

---

## `rs.gh` — GitHub CLI operations

Used for exercises that test remote repository behavior.

```python
rs.gh.repo_create(owner=None, repo="my-repo", public=True)
rs.gh.repo_clone(owner="git-mastery", repo="exercises", directory="local-dir")
rs.gh.repo_fork(owner="git-mastery", repo="exercises", clone=True)
rs.gh.repo_delete(owner=None, repo="my-repo")
rs.gh.repo_view(owner="git-mastery", repo="exercises")
```

---

## Custom helpers

Custom helpers extend `RepoSmith` with domain-specific operations. Subclass `Helper` and register it before use:

```python
from git import Repo
from repo_smith.helpers.helper import Helper


class GitMasteryHelper(Helper):
    def __init__(self, repo: Repo, verbose: bool) -> None:
        super().__init__(repo, verbose)

    def create_start_tag(self) -> None:
        """Creates the git-mastery-start-<hash> tag required by git-autograder."""
        all_commits = list(self.repo.iter_commits())
        first_commit = list(reversed(all_commits))[0]
        tag = f"git-mastery-start-{first_commit.hexsha[:7]}"
        self.repo.create_tag(tag)


# Register once on the RepoSmith instance, then call via rs.helper(...)
rs.add_helper(GitMasteryHelper)
rs.helper(GitMasteryHelper).create_start_tag()
```

{: .note }
`GitMasteryHelper` is already defined in `exercise_utils.test` and is registered automatically when using `GitAutograderTestLoader`. You only need to implement it yourself if you are calling `create_repo_smith` directly outside of exercises.

---

## Typical test pattern in exercises

Tests in `exercises` do not call `create_repo_smith` directly. Instead they use `GitAutograderTestLoader` from `exercise_utils.test`, which sets up the exercise environment and provides `rs` automatically.

### Basic test

```python
from exercise_utils.test import GitAutograderTestLoader, GitMasteryHelper, assert_output
from git_autograder import GitAutograderStatus
from .verify import verify, NO_MERGES, MISSING_MERGES

REPOSITORY_NAME = "branch-bender"

loader = GitAutograderTestLoader(REPOSITORY_NAME, verify)


def test_successful_merge() -> None:
    with loader.start() as (test, rs):
        rs.git.commit(message="Initial commit", allow_empty=True)
        rs.helper(GitMasteryHelper).create_start_tag()

        rs.git.checkout("feature/login", branch=True)
        rs.git.commit(message="Add login", allow_empty=True)
        rs.git.checkout("main")
        rs.git.merge("feature/login", no_ff=True)

        output = test.run()
        assert_output(output, GitAutograderStatus.SUCCESSFUL)


def test_no_merges() -> None:
    with loader.start() as (test, rs):
        rs.git.commit(message="Initial commit", allow_empty=True)
        rs.helper(GitMasteryHelper).create_start_tag()

        output = test.run()
        assert_output(output, GitAutograderStatus.UNSUCCESSFUL, [NO_MERGES])
```

### With a remote repository

```python
def test_remote_branch_rename() -> None:
    with loader.start(include_remote_repo=True) as (test, rs, rs_remote):
        remote_path = str(rs_remote.repo.git_dir)

        rs.git.commit(message="Initial commit", allow_empty=True)
        rs.git.remote_add("origin", remote_path)
        rs.git.branch("feature/old")
        rs.git.push("origin", "feature/old")

        # Simulate the student's action
        rs.git.branch("feature/new", old_branch="feature/old", move=True)
        rs.git.push("origin", "feature/new")
        rs.git.push("origin", ":feature/old")

        output = test.run()
        assert_output(output, GitAutograderStatus.SUCCESSFUL)
```

### Starting from a cloned repository

```python
def test_from_clone() -> None:
    with loader.start(clone_from="https://github.com/git-mastery/some-repo") as (test, rs):
        output = test.run()
        assert_output(output, GitAutograderStatus.SUCCESSFUL)
```

{: .reference }

See [Testing guide](/developers/docs/exercises/testing-patterns) for the full `GitAutograderTestLoader` API.
