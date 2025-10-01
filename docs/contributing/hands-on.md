---
title: Hands-on
parent: Contributing
nav_order: 2
---

1. TOC
{:toc}

## What is a hands-on?

[Lessons accompanying Git-Mastery App](https://nus-cs2103-ay2526s1.github.io/website/se-book-adapted/git-trail/index.html) contains hands-on practicals for students to get 'hands-on' experience of the Git concepts covered by the lesson. Some of those hands-on practicals need to set up a 'sandbox' containing the required folders, files, repos before the student can do the practical. Git-Mastery app can set up that sandbox for students so that they can get to the 'practical' part more easily (we refer to this as 'downloading' the hands-on), without having to set up the sandbox manually. For example, a student can run `gitmastery download <hands-on name>` (e.g., `gitmastery download hp-init-repo`) to set up the sandbox for a specific hands-on practical.

Some examples of how Git-Mastery app helps to create the sandbox for a hands-on practical can be seen in [this Git Tour](https://nus-cs2103-ay2526s1.github.io/website/book/gitAndGithub/trail/recordingFolderHistory/index.html) (see T1L3, T1L4).

## Before contributing

New contributors are recommended to start by implementing an [already approved hands-on proposal]((https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22hands-on%20discussion%22%20label%3A%22help%20wanted%22)).

If you are proposing a new hands-on instead, make sure that you have done the following:

- [ ] Create an [hands-on discussion](https://github.com/git-mastery/exercises/issues/new?template=hands_on_discussion.yaml)
- [ ] Obtain approval on the hands-on
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Implementing a hands-on

First, run the provided `new.sh` script to generate the scaffolding for a new hands-on sandbox:

```bash
./new.sh
```

The script will first prompt if you want to create a hands-on or exercise:

Enter `hands-on` or `h` to create a new hands-on.

Then, the script will prompt you for:

1. The name of the hands-on -- likely to be specified in the corresponding hands-on discussion (without the `hp-` prefix!) (you should be using kebab case for the hands-on name)
2. Does the hands-on require Git?
3. Does the hands-on require Github?

{: .reference }

Refer to the [hands-on structure document](/developers/docs/architecture/hands-on-structure) for more information about the folder structure generated.


Each hands-on is implemented as a single file `hands_on/<hands-on name>.py`, containing the instructions to set up the hands-on sandbox .

{: .reference }

For more information about how Git-Mastery downloads a hands-on, refer to the [Download Workflow](/developers/docs/architecture/download-workflow)

{: .note }

> `exercises` repo comes with a set of utility functions in the `exercise_utils` module that are made available during the download flow. They provide simple wrappers around common functionality such as `exercise_utils.cli.run_command` to invoke any command and `exercise_utils.file.create_or_update_file` to create or update a given file.
>
> For the full list of utility functions, refer [here](/developers/docs/tooling/exercise-utils).

These are some references for download setups for other exercises:

- [init-repo](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/init_repo.py)
- [add-files](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/add_files.py)
- [stage-modified](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/stage_modified.py)

### Conventions

1. Any operations should use OS-agnostic options (e.g. opting to use `shutil.rmtree` over `run_command(["rm"])`)
2. For any commands that require `gh` (GitHub CLI), make sure that the `__requires_github__` variable in the download setup is set to `True` so that the app will automatically check that the student has correctly setup Github CLI

### Testing downloads

To test that your download script works, we have provided a script to simulate the download process in this repository for you to verify.

```bash
./test-download.sh <hands-on name>
```

You can find the sandbox created by the script under the `test-downloads/` folder. Check it manually to verify it is as expected.

## Submitting the hands-on for review

Create a pull request from your fork using the provided pull request template.

Fill in all the details necessary.

## Example

1. Hands-on discussion: <https://github.com/git-mastery/exercises/issues/44>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Hands-on PR: <https://github.com/git-mastery/exercises/pull/45>

