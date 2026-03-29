---
title: Configuration
parent: App
nav_order: 2
---

# Configuration

The app uses local configuration files to track Git-Mastery setup and exercise metadata.

## Important files

- `.gitmastery.json`: local Git-Mastery configuration
- `.gitmastery-exercise.json`: metadata for a downloaded exercise

## `.gitmastery.json`

This file lives at the Git-Mastery root created by `gitmastery setup`.

### Main fields

- `progress_local`: whether local progress tracking is enabled
- `progress_remote`: whether progress sync to GitHub is enabled
- `exercises_source`: where the app should fetch exercises from

### `exercises_source`

The app supports both remote and local exercise sources.

Remote source shape:

```json
{
  "type": "remote",
  "username": "git-mastery",
  "repository": "exercises",
  "branch": "main"
}
```

Local source shape:

```json
{
  "type": "local",
  "repo_path": "/absolute/path/to/exercises"
}
```

When `type` is `local`, the app copies the local exercises repository into a temporary directory and reads exercises from there. This is useful when developing `app` and `exercises` together.

### Local co-development example

If you are working on `app` and want it to use a local checkout of `exercises`, update `.gitmastery.json` like this:

```json
{
  "progress_local": true,
  "progress_remote": false,
  "exercises_source": {
    "type": "local",
    "repo_path": "/Users/you/dev/exercises"
  }
}
```

This lets `gitmastery download` and `gitmastery verify` read exercise content from your local `exercises` clone instead of GitHub.

### Remote source override example

You can also point the app at a fork or branch of `exercises`:

```json
{
  "progress_local": true,
  "progress_remote": false,
  "exercises_source": {
    "type": "remote",
    "username": "<your-username>",
    "repository": "exercises",
    "branch": "feature/my-branch"
  }
}
```

## `.gitmastery-exercise.json`

This file lives in each downloaded exercise root.

### Main fields

- `exercise_name`
- `tags`
- `requires_git`
- `requires_github`
- `base_files`
- `exercise_repo`
- `downloaded_at`

### `exercise_repo`

The app currently understands these repository modes:

- `local`
- `remote`
- `ignore`
- `local-ignore`

These fields control whether the app creates a local repository, clones a remote repository, skips repository creation, or creates a folder without initializing Git.

For remote exercises, the app may also read `fork_all_branches` when creating a student fork.

## How the app uses these files

- `setup` writes `.gitmastery.json`
- `download` reads the Git-Mastery root config, then writes or updates `.gitmastery-exercise.json`
- `verify` reads the exercise config to determine how the exercise should be graded
- `progress` reads the Git-Mastery root config to determine whether local or remote sync is enabled
