---
title: Exercise structure
parent: Architecture
nav_order: 2
---

# Exercise structure

Exercises follow a fixed structure for the Git-Mastery app to pick up on:

```text
<exercise name>
├── .gitmastery-exercise.json
├── README.md
├── __init__.py
├── download.py
├── res
│   └── ...
├── tests
│   ├── specs
│   │   └── base.yml
│   └── test_verify.py
└── verify.py
```

- `.gitmastery-exercise.json`: contains the exercise configuration
- `README.md`: contains the instructions for the exercise for the students to attempt
- `download.py`: contains the download instructions to setup the student's exercise
- `verify.py`: contains the verification script for the exercise attempt
- `res/`: contains resources that are available to students (see this section about [types of resources](#types-of-resources))
- `tests/specs/`: contains specification files written using [`repo-smith`](https://github.com/git-mastery/git-autograder)
- `tests/test_verify.py`: contains unit tests for verification script

## What students see

When a student downloads an exercise, they will see the following folder structure:

```text
<exercise name>
├── .gitmastery-exercise.json
├── README.md
└── <sub folder name>
    ├── .git
    └── ...
```

The root of the exercise will contain the `README.md` and `.gitmastery-exercise.json` configured from your template.

It also contains the sub-folder configured in `.gitmastery-exercise.json`, which is where students will attempt the exercise.

## Configuration structure

`.gitmastery-exercise.json` is used to tell the [Git-Mastery app](https://git-mastery.github.io/app) how to setup the student's exercise.

{: .note }

We opted to use a standardized configuration for exercises because they often follow a certain shape for being setup so it is easier if the application
standardizes the setup via the exercise configuration.

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

## Exercise resource types

There are two distinct types of resources:

1. Base files: configured through the `base_files` property in `.gitmastery-exercise.json` in your template; files located in `res/` are downloaded to the root of the exercise folder

    ```text
    <exercise name>
    ├── .gitmastery-exercise.json
    ├── README.md
    ├── <base files> <-- here
    └── <sub folder name>
        ├── .git
        └── ...
    ```

2. Resources: configured through the `__resources__` field in `download.py`; supporting files for the exercise with files located in `res/` downloaded into the sub folder

    ```text
    <exercise name>
    ├── .gitmastery-exercise.json
    ├── README.md
    ├── <base files>
    └── <sub folder name>
        ├── .git
        └── <resources> <-- here
    ```
