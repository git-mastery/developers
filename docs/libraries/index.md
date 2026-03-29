---
title: Shared libraries
nav_order: 6
has_children: true
---

# Shared libraries

Git-Mastery maintains reusable libraries that support exercise verification and testing.

These libraries are most relevant when you are working on reusable grading behavior rather than a single exercise.

## Main libraries

- [`repo-smith`](https://github.com/git-mastery/repo-smith): repository construction for unit tests
- [`git-autograder`](https://github.com/git-mastery/git-autograder): grading helpers and verification abstractions

## When to use which library

- Reach for `repo-smith` when your test needs a specific repository history or working tree state.
- Reach for `git-autograder` when multiple exercises would benefit from the same verification helper or abstraction.

## Scope of this section

This section focuses on the two shared libraries that new Git-Mastery contributors are most likely to touch while working on verification behavior or test infrastructure.
