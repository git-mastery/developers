---
title: Configuration reference
parent: App
nav_order: 2
---

# Configuration reference

The app uses two JSON files to manage state: one for the Git-Mastery workspace and one per downloaded exercise.

## `.gitmastery.json`

Created by `gitmastery setup` in the Git-Mastery root directory.

| Field              | Type     | Description                                 |
| ------------------ | -------- | ------------------------------------------- |
| `progress_local`   | `bool`   | Whether local progress tracking is enabled  |
| `progress_remote`  | `bool`   | Whether progress is synced to a GitHub fork |
| `exercises_source` | `object` | Where the app fetches exercises from        |

### `exercises_source`

Two source types are supported:

**Remote** (default):

```json
{
  "type": "remote",
  "username": "git-mastery",
  "repository": "exercises",
  "branch": "main"
}
```

**Local** (for co-developing `app` and `exercises`):

```json
{
  "type": "local",
  "repo_path": "/absolute/path/to/exercises"
}
```

With `type: local`, the app copies the local exercises directory into a temp directory at runtime. This means changes you make in your local `exercises/` clone are picked up immediately without needing to push to GitHub — useful when developing `app` and `exercises` together.

To point at a fork or a feature branch, change `username` or `branch` in the remote config.

---

## `.gitmastery-exercise.json`

Created per exercise by `gitmastery download`. Lives in the exercise root.

| Field             | Type       | Description                                                                                                             |
| ----------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| `exercise_name`   | `string`   | Exercise identifier used for progress tracking                                                                          |
| `tags`            | `string[]` | Used for indexing on the exercise directory                                                                             |
| `requires_git`    | `bool`     | If true, app checks Git installation and `user.name`/`user.email` before download                                       |
| `requires_github` | `bool`     | If true, app checks GitHub CLI authentication before download                                                           |
| `base_files`      | `object`   | Map of destination filename to source path in `res/`; these files are copied to the exercise root alongside `README.md` |
| `exercise_repo`   | `object`   | Controls what working repository the student receives                                                                   |
| `downloaded_at`   | `string`   | ISO timestamp of when the exercise was downloaded                                                                       |

### `exercise_repo`

| Field               | Type     | Description                                                  |
| ------------------- | -------- | ------------------------------------------------------------ |
| `repo_type`         | `string` | One of `local`, `remote`, `ignore`, `local-ignore`           |
| `repo_name`         | `string` | Name of the subfolder created for the student                |
| `init`              | `bool`   | Whether to run `git init` (used with `local`)                |
| `create_fork`       | `bool`   | Whether to fork the remote repo to the student's account     |
| `fork_all_branches` | `bool`   | Whether all branches are included in the fork (remote only)  |
| `repo_title`        | `string` | Name of the GitHub repository to clone or fork (remote only) |

### `repo_type` values

| Value          | Behaviour                                                       |
| -------------- | --------------------------------------------------------------- |
| `local`        | Creates a local folder, optionally runs `git init`              |
| `remote`       | Clones or fork-and-clones a GitHub repository                   |
| `ignore`       | No repository created; `exercise.repo` is a null wrapper        |
| `local-ignore` | Creates a folder without `git init`; student runs it themselves |

## How commands use these files

| Command    | Reads                       | Writes                              |
| ---------- | --------------------------- | ----------------------------------- |
| `setup`    | —                           | `.gitmastery.json`                  |
| `download` | `.gitmastery.json`          | `.gitmastery-exercise.json`         |
| `verify`   | `.gitmastery-exercise.json` | `progress/progress.json`            |
| `progress` | `.gitmastery.json`          | `.gitmastery.json` (on sync toggle) |
