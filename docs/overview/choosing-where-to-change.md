---
title: Choosing where to change
parent: Overview
nav_order: 2
---

# Choosing where to change

When a feature or bug spans multiple repositories, it helps to decide where the primary change belongs.

## Change `exercises` when

- the exercise instructions are wrong
- a hands-on or exercise setup needs different files or repository state
- a `verify.py` rule is exercise-specific
- a test for a single exercise needs updating

## Change `app` when

- the CLI command behavior is wrong
- download, verify, setup, or progress flows need to change for all users
- `.gitmastery.json` or `.gitmastery-exercise.json` handling needs to change
- the exercises source behavior or local-development override behavior needs to change

## Change `git-autograder` when

- multiple exercises need the same verification helper
- `GitAutograderExercise`, output handling, answer parsing, or repository wrappers need improvement
- grading behavior should become easier to express across many exercises

## Change `repo-smith` when

- tests need a repository state that is hard to express today
- a new YAML step type or hook capability would simplify many exercise tests
- repository construction logic should be reusable beyond a single exercise

## Typical examples

- A bug in one exercise's branch-order check usually belongs in `exercises`.
- A missing helper for parsing `answers.txt` usually belongs in `git-autograder`.
- A missing repository setup primitive for tests usually belongs in `repo-smith`.
- A bug in `gitmastery progress sync on` belongs in `app`.

## Rule of thumb

If the change is specific to one learning task, start in `exercises`.

If the change improves the platform behavior for many exercises or many contributors, it probably belongs in `app`, `git-autograder`, or `repo-smith`.
