---
title: Exercise format reference
parent: Exercises
nav_order: 3
---

# Exercise format reference

Exercises follow a fixed structure for the Git-Mastery app to pick up on:

```text
<exercise name>
├── .gitmastery-exercise.json
├── README.md
├── __init__.py
├── download.py
├── res
│   └── ...
├── test_verify.py
└── verify.py
```

- `.gitmastery-exercise.json`: contains the exercise configuration
- `README.md`: contains the instructions for students
- `download.py`: contains the download instructions to set up the student's exercise
- `verify.py`: contains the verification script for the exercise attempt
- `res/`: contains resources that are available to students
- `test_verify.py`: contains unit tests for the verification script, written using [`repo-smith`](https://github.com/git-mastery/repo-smith)

## What students see

When a student downloads a typical exercise, they will see the following folder structure:

```text
<exercise name>
├── .gitmastery-exercise.json
├── README.md
└── <sub folder name>
    ├── .git
    └── ...
```

The root of the exercise contains the configured `README.md` and `.gitmastery-exercise.json`.

It also contains the subfolder configured in `.gitmastery-exercise.json`, which is where students attempt the exercise.

Some exercises intentionally differ:

- `repo_type: ignore`: no managed repository subfolder is created by the app; the student works directly in the exercise root.
- `repo_type: local-ignore`: the app creates the working folder but does not run `git init` for the student.

## Configuration structure

`.gitmastery-exercise.json` is used to tell the [Git-Mastery app](https://git-mastery.github.io/app) how to set up the student's exercise.

{: .note }

We opted to use a standardized configuration for exercises because they often follow a common setup shape.

The `new.sh` script generates one for you, but you can modify the file directly:

- `exercise_name`: raw exercise name that will be indexed; recommended to use [kebab case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case)
- `tags`: used during indexing on the [exercise directory](https://git-mastery.github.io/exercises)
- `requires_git`: performs a check to ensure that Git is installed and `user.name` and `user.email` are configured
- `requires_github`: performs a check to ensure that GitHub CLI is installed and the user has authenticated
- `base_files`: specifies the files from `res/` to be downloaded into the exercise root
- `exercise_repo`: controls the subfolder that is generated; this is where students work on the exercise
  - `repo_type`: one of `local`, `remote`, `ignore`, or `local-ignore`
  - `repo_name`: name of the subfolder
  - `init`: determines if `git init` is run for the subfolder; used for local or local-ignore style repositories
  - `create_fork`: determines if a fork is created on the user's GitHub account; used for remote repositories
  - `fork_all_branches`: optional remote-only setting controlling whether all branches are forked
  - `repo_title`: name of the remote repository to fork and clone; used for remote repositories

### Repo type meanings

- `local`: create a local subfolder and optionally initialize it as a Git repository.
- `remote`: clone a GitHub repository, optionally via a student fork.
- `ignore`: do not create or manage a working repository; verification should not depend on `exercise.repo`.
- `local-ignore`: create a local subfolder but do not initialize Git. This is useful for exercises where the student is expected to run `git init` themselves.

## Exercise resource types

There are two distinct types of resources:

1. Base files: configured through the `base_files` property in `.gitmastery-exercise.json`; files located in `res/` are downloaded to the root of the exercise folder.

    ```text
    <exercise name>
    ├── .gitmastery-exercise.json
    ├── README.md
    ├── <base files>
    └── <sub folder name>
        ├── .git
        └── ...
    ```

2. Resources: configured through the `__resources__` field in `download.py`; supporting files from `res/` are downloaded into the subfolder.

    ```text
    <exercise name>
    ├── .gitmastery-exercise.json
    ├── README.md
    ├── <base files>
    └── <sub folder name>
        ├── .git
        └── <resources>
    ```
