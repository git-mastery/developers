---
title: Developer tooling
nav_order: 3
parent: Tooling
---

## Publishing new versions of libraries

When working on `app` or the other supported libraries, if you attach one of the following labels:

- `bump:major`
- `bump:minor`
- `bump:patch`

The CI will automatically tag the merge commit when the pull request is merged and automatically perform a publish for the library/tool.
