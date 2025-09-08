---
title: Deployment Pipeline
parent: Architecture
nav_order: 5
---

## Exercises

Unit tests and [exercise configuration](/developers/docs/architecture/exercise-structure/#configuration-structure) will be run during the PR and every time a branch is merged into `main`.

These are controlled through the `CI` action on Github Actions ([here](https://github.com/git-mastery/exercises/actions/workflows/ci.yml)).

## Git-Mastery app

A new version of the Git-Mastery application is published when a new tag is pushed to `main`.

The versioning is done as follows: `<major>.<minor>.<patch>` where

1. `<major>`: for when major changes to the app occur (i.e. rewrites) where the application could break entirely
2. `<minor>`: for when critical bug fixes occur that users should be aware of
3. `<patch>`: for when minor bug fixes occur (i.e. cosmetic) that users don't need to update to

The app automatically checks for the latest version published and warns users if their local version is out of date if the `<major>` or `<minor>` versions are wrong.

If you do not have permissions to push a new tag, contact <woojiahao1234@gmail.com> for assistance.
