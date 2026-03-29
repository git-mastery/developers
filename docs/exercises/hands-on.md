---
title: Hands-on
parent: Exercises
nav_order: 1
---

1. TOC
{:toc}

## What is a hands-on?

[Lessons accompanying Git-Mastery App](https://nus-cs2103-ay2526s1.github.io/website/se-book-adapted/git-trail/index.html) contains hands-on practicals for students to get hands-on experience of the Git concepts covered by the lesson. Some of those hands-on practicals need to set up a sandbox containing the required folders, files, and repositories before the student can start. Git-Mastery app can set up that sandbox for students so that they can get to the practical part more easily.

For example, a student can run `gitmastery download hp-init-repo` to set up a sandbox for a specific hands-on practical.

Some examples of how Git-Mastery app helps create the sandbox for a hands-on practical can be seen in [this Git Tour](https://nus-cs2103-ay2526s1.github.io/website/book/gitAndGitHub/trail/recordingFolderHistory/index.html) (see T1L3 and T1L4).

## Before contributing

New contributors are recommended to start by implementing an [already approved hands-on proposal](https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22hands-on%20discussion%22%20label%3A%22help%20wanted%22).

If you are proposing a new hands-on instead, make sure that you have done the following:

- [ ] Create a [hands-on discussion](https://github.com/git-mastery/exercises/issues/new?template=hands_on_discussion.yaml)
- [ ] Obtain approval on the hands-on
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Implementing a hands-on

First, run the provided `new.sh` script to generate the scaffolding for a new hands-on sandbox:

```bash
./new.sh
```

The script will first prompt if you want to create a hands-on or exercise.

Enter `hands-on` or `h` to create a new hands-on.

Then, the script will prompt you for:

1. The name of the hands-on, likely specified in the corresponding hands-on discussion, without the `hp-` prefix. Use kebab case.
2. Whether the hands-on requires Git
3. Whether the hands-on requires GitHub

{: .reference }

Refer to the [Hands-on structure](/developers/docs/exercises/hands-on-structure) page for more information about the generated file structure.

Each hands-on is implemented as a single file at `hands_on/<hands-on name>.py`, containing the instructions to set up the hands-on sandbox.

{: .reference }

For more information about how Git-Mastery downloads a hands-on, refer to the [Download workflow](/developers/docs/exercises/download-workflow).

{: .note }

> `exercises` comes with a set of utility functions in the `exercise_utils` module that are made available during the download flow. They provide simple wrappers around common functionality such as `exercise_utils.cli.run_command` to invoke commands and `exercise_utils.file.create_or_update_file` to create or update files.
>
> For the full list of utility functions, refer to [Exercise utilities](/developers/docs/exercises/exercise-utils).

These are some references for download setups for other hands-ons:

- [init-repo](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/init_repo.py)
- [add-files](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/add_files.py)
- [stage-modified](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/stage_modified.py)

### Conventions

1. Any operations should use OS-agnostic options, for example `shutil.rmtree` instead of `run_command(["rm"])`.
2. For any commands that require `gh`, make sure that the `__requires_github__` variable in the download setup is set to `True` so that the app automatically checks that the student has set up GitHub CLI correctly.

### Testing downloads

To test that your download script works, use the provided script:

```bash
./test-download.sh <hands-on name>
```

You can find the sandbox created by the script under `test-downloads/`. Check it manually to verify that it is as expected.

## Submitting the hands-on for review

Create a pull request from your fork using the provided pull request template.

## Example

1. Hands-on discussion: <https://github.com/git-mastery/exercises/issues/44>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Hands-on PR: <https://github.com/git-mastery/exercises/pull/45>
