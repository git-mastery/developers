---
title: Exercise configuration structure
parent: Architecture
---

### `.gitmastery-exercise.json` configuration

The `.gitmastery-exercise.json` is used to tell the [Git-Mastery app](https://git-mastery.github.io/app) how to setup the student's exercise.

The `new.sh` script should have already generated one for you, but you may change your mind with the configuration and modify the file directly:

- `exercise_name`: raw exercise name that will be indexed; recommended to use [kebab case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case)
- `tags`: used during indexing on the [exercise directory](https://git-mastery.github.io/exercises)
- `requires_git`: performs a check to ensure that Git is installed and the user has already configured their `user.name` and `user.email`
- `requires_github`: performs a check to ensure that Github CLI is installed and the user has already authenticated themselves
- `base_files`: specifies the files from `res/` to be downloaded into the exercise root; typically used for the `answers.txt` (more about grading types [here]())
- `exercise_repo`: controls the sub-folder that is generated; this is where students work on the exercise
  - `repo_type`: `local` or `remote`; if `remote`, then the sub-folder is generated from a remote repository
  - `repo_name`: name of the sub-folder; required for both `repo_type`
  - `init`: determines if `git init` is run for the sub-folder; required only for `local`
  - `create_fork`: determines if a fork is created on the user's Github account; required only for `remote`
  - `repo_title`: name of the remote repository to fork + clone; required only for `remote`
