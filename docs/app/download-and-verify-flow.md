---
title: Download and verification flow
parent: App
nav_order: 3
---

# Download and verification flow

This page explains how the `gitmastery` CLI coordinates exercise content from `exercises` with runtime behavior in `app`.

## Download flow

When a user runs `gitmastery download <name>`, the app first determines whether `<name>` is a hands-on or an exercise.

### Hands-ons

For hands-ons, the app:

1. checks that the matching `hands_on/*.py` file exists in the exercises source
2. creates a fresh local folder
3. loads the hands-on file as a Python namespace
4. checks Git and GitHub prerequisites if `__requires_git__` or `__requires_github__` are set
5. executes `download(...)`

The hands-on script receives a `repo-smith` helper object as `rs`, which is useful for file and repository setup without needing a fully managed student repository.

### Exercises

For exercises, the app:

1. checks that `.gitmastery-exercise.json` exists in the exercises source
2. creates a fresh exercise root directory
3. downloads the base files `.gitmastery-exercise.json` and `README.md`
4. loads the exercise config locally
5. checks Git and GitHub prerequisites when required
6. downloads any configured `base_files`
7. sets up the working repository or working folder according to `exercise_repo.repo_type`
8. loads `download.py` and executes `setup(...)`

## Working repository modes

The `exercise_repo.repo_type` field changes how download behaves:

- `local`: create a local working folder, optionally initialize Git, then run `setup(...)`
- `remote`: clone a GitHub repository or fork-and-clone it, then run `setup(...)`
- `ignore`: skip managed repository creation and leave the student at the exercise root
- `local-ignore`: create a working folder but do not run `git init`

## Verify flow

When a user runs `gitmastery verify`, the app:

1. confirms that the current directory belongs to a downloaded exercise
2. loads the local exercise config
3. fetches `verify.py` from the configured exercises source
4. constructs `GitAutograderExercise`
5. executes `verify(exercise)`
6. converts wrong-answer and invalid-state exceptions into structured output
7. prints the result to the terminal
8. updates local progress and optionally remote progress

## Where code lives

- `app/app/commands/download.py`: download orchestration
- `app/app/commands/verify.py`: verify orchestration and progress submission
- `app/app/utils/gitmastery.py`: exercises source loading and dynamic namespace execution
- `exercises/<exercise>/download.py`: exercise-specific setup logic
- `exercises/<exercise>/verify.py`: exercise-specific grading logic

## Why this split exists

- `app` owns runtime behavior, safety checks, and student-facing orchestration
- `exercises` owns content and exercise-specific logic
- `git-autograder` owns reusable verification abstractions
- `repo-smith` supports deterministic repository setup for tests and downloads
