---
title: repo-smith reference
parent: Shared libraries
nav_order: 1
---

# repo-smith reference

`repo-smith` is a YAML-based library for initializing Git repositories for unit testing.

## Typical use in Git-Mastery

- Build repository states for `test_verify.py`
- Simulate specific Git histories and repository layouts
- Keep verification tests readable through declarative setup

## Example usage

```python
from repo_smith.initialize_repo import initialize_repo


def test_dummy():
    repo_initializer = initialize_repo("tests/specs/basic_spec.yml")
    with repo_initializer.initialize() as repo:
        print(repo)
```

## Why Git-Mastery uses it

`repo-smith` keeps exercise verification tests declarative. Instead of building test repositories imperatively in Python, contributors can describe the repository history and filesystem state in YAML.

## Spec structure

A spec contains:

- optional `name`
- optional `description`
- `initialization`
  - optional `clone-from`
  - ordered `steps`

All steps run sequentially from top to bottom.

## Supported step types

Current step types include:

- `commit`
- `add`
- `tag`
- `new-file`
- `edit-file`
- `delete-file`
- `append-file`
- `bash`
- `branch`
- `branch-rename`
- `branch-delete`
- `checkout`
- `remote`
- `reset`
- `revert`
- `merge`
- `fetch`

## Features worth knowing

### `clone-from`

Use `initialization.clone-from` when the test should begin from an existing repository instead of an empty freshly initialized one.

### Step IDs and hooks

When a step has an `id`, tests can attach pre-hooks and post-hooks around that specific step.

This is useful when you need to assert intermediate repository state, not just the final result.

```python
from repo_smith.initialize_repo import initialize_repo


def test_with_hook() -> None:
    repo_initializer = initialize_repo("tests/specs/hooks.yml")

    def before_commit(repo) -> None:
        print(repo.working_tree_dir)

    repo_initializer.add_pre_hook("first-commit", before_commit)

    with repo_initializer.initialize() as repo:
        print(repo)
```

## Typical Git-Mastery test pattern

1. Write one or more YAML specs under `tests/specs/`
2. Build the repository state with `initialize_repo(...)`
3. Run the exercise's `verify(...)` function against that state
4. Assert on comments and final status

{: .reference }

See [Testing guide](/developers/docs/exercises/testing-patterns) for how this is typically wrapped inside `exercises` tests.

For the full field-level specification, refer to `repo-smith/specification.md` in the [`repo-smith` repository](https://github.com/git-mastery/repo-smith).
