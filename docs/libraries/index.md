---
title: Libraries
nav_order: 6
has_children: true
---

# Libraries

Git-Mastery maintains reusable libraries that support exercise verification and testing.

These libraries are most relevant when you are working on reusable grading behavior rather than a single exercise.

## Main libraries

- [`repo-smith`](https://github.com/git-mastery/repo-smith): repository construction for unit tests
- [`git-autograder`](https://github.com/git-mastery/git-autograder): grading helpers and verification abstractions

## When to use which library

- Reach for `repo-smith` when your test needs a specific repository history or working tree state.
- Reach for `git-autograder` when multiple exercises would benefit from the same verification helper or abstraction.

{: .reference }

For a broader decision guide across repositories, see [Choosing where to change](/developers/docs/overview/choosing-where-to-change).

## Additional published packages

- [`difflib-parser`](https://github.com/git-mastery/difflib-parser)
