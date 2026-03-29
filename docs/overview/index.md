---
title: Overview
nav_order: 2
has_children: true
---

# Overview

Git-Mastery is a multi-repository project, and the documentation is organized around the different repositories and components. This overview page serves as a map to the ecosystem, guiding you to the relevant documentation for each part of the project.

## Core repositories

These are the main repositories that make up the Git-Mastery ecosystem, and are the most relevant for contributors:

- [`exercises`](https://github.com/git-mastery/exercises): hands-ons, exercises, download setup, and verification tests
- [`app`](https://github.com/git-mastery/app): the `gitmastery` CLI used by students and contributors
- [`repo-smith`](https://github.com/git-mastery/repo-smith): test-support library that creates Git repository states through helper objects so exercise verification scenarios can be set up deterministically in tests
- [`git-autograder`](https://github.com/git-mastery/git-autograder): grading library that loads a Git-Mastery exercise attempt and turns repository or answer checks into structured verification result

## Supporting repositories

These repositories are part of the wider ecosystem and may receive dedicated documentation later:

- [`progress-dashboard`](https://github.com/git-mastery/progress-dashboard): visualizes learner progress data
- [`difflib-parser`](https://github.com/git-mastery/difflib-parser): library for parsing diff output into structured data

## Where to start

- New contributor: start with [Getting started](/developers/docs/getting-started)
- Exercise contributor: go to [Exercises](/developers/docs/exercises)
- CLI contributor: go to [App](/developers/docs/app)
- Library contributor: go to [Shared libraries](/developers/docs/libraries)
