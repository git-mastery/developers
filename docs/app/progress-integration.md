---
title: Progress tracking
parent: App
nav_order: 4
---

# Progress tracking

The app is responsible for writing and syncing exercise progress based on verification results.

Progress data is part of the wider Git-Mastery ecosystem and can be consumed by other repositories such as `progress-dashboard`.

## Local progress

`gitmastery setup` creates a `progress/` folder inside the Git-Mastery root.

After each `gitmastery verify` run, the app appends a new entry to `progress/progress.json` with:

- `exercise_name`
- `started_at`
- `completed_at`
- `comments`
- `status`

The app keeps a history of attempts, but `gitmastery progress show` displays the latest known entry for each exercise.

## Remote sync

When a user runs `gitmastery progress sync on`, the app:

1. Checks Git and GitHub CLI prerequisites
2. Creates or reuses the user's fork of the progress repository
3. Replaces the local `progress/` folder with a clone of that fork
4. Merges local and remote progress entries
5. Pushes changes and opens a PR if needed

When remote sync is enabled, later `verify` and `progress reset` runs also update the remote fork.

## Turning sync off

`gitmastery progress sync off` deletes the remote progress fork, switches `.gitmastery.json` back to local-only tracking, and recreates a plain local `progress/` folder containing the existing progress entries.

## Reset behavior

`gitmastery progress reset` does two things for the current exercise:

- recreates the exercise workspace
- removes saved progress entries for that exercise from `progress.json`

{: .reference }

See [Download and verification flow](/developers/docs/app/download-and-verify-flow) for the command orchestration that happens before progress is updated.
