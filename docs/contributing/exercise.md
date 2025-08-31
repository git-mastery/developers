---
title: Exercise
parent: Contributing
nav_order: 2
---

1. TOC
{:toc}

## Before contributing

If you are proposing a new exercise (i.e., not implementing an [already approved exercise proposal](https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22exercise%20discussion%22%20label%3A%22help%20wanted%22)) make sure that you have done the following:

- [ ] Create an [exercise discussion](https://github.com/git-mastery/exercises/issues/new?template=exercise_discussion.yaml)
- [ ] Obtained approval on the exercise
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Create a new exercise

Use the provided `new.sh` script to generate the scaffolding for a new exercise:

```bash
./new.sh
```

The script will first prompt if you want to create a hands-on or exercise:

Enter `exercise` or `e` to create a new exercise.

Then, the script will prompt you for:

1. The name of the exercise -- likely to be specified in the corresponding exercise discussion (you should be using kebab case for the exercise name)
2. The exercise tags (split by space) -- likely to be specified in the corresponding exercise discussion
3. The exercise configuration (read the [exercise configuration](/developers/docs/architecture/exercise-structure#configuration-structure) section for more info on this)

{: .reference }

Refer to the [exercise structure document](/developers/docs/architecture/exercise-structure) for more information about the folder structure generated.

## Download setup

The `download.py` contains the instructions to setup the local repository.

{: .reference }

For more information about how Git-Mastery downloads exercises, refer to the [Download Workflow](/developers/docs/architecture/download-workflow)

The default download script comes with a helper function to `run_command` to run local command, and a command to attach a tag as the "start tag". This is only useful if you want to iterate through the user's commits in your verification script. Otherwise, this can be removed.

These are some references for download setups for other exercises:

- [ignoring-somethings](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/ignoring_somethings/download.py)
- [branch-bender](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/branch_bender/download.py)

### Download conventions

1. Any operations should use OS agnostic options (e.g. opting to use `shutil.rmtree` over `run_command(["rm"])`)
2. If you need to compare the states before and after the student has started to add commits, use the given "start tag" as `git-autograder` is designed to read that if necessary
3. For any commands that require `gh` (Github CLI), make sure that the `requires_github` configuration is set to `true` so that the app will automatically check that the student has correctly setup Github CLI

### Testing downloads

To test that your download script works, we have provided a script to simulate the download process in this repository for you to verify.

```bash
./test-download.sh <your exercise folder>
```

You can find the downloaded repository under the `test-downloads/` folder.

## Verification setup

The verification process is controlled by the `verify.py`.

{: .reference }

For more information about how Git-Mastery verifies exercise attempts, refer to the [Verification Workflow](/developers/docs/architecture/verification-workflow)

The [`git-autograder`](https://github.com/git-mastery/git-autograder) is built as a wrapper around [`GitPython`](https://github.com/gitpython-developers/GitPython). As a result, if you are writing any verification scripts and there is no available helper function with `git-autograder`, you can fall back to the underlying `Repo` object:

```python
def verify(exercise: GitAutograderExercise) -> GitAutograderOutput:
    # Access the underlying GitPython repo:
    exercise.repo.repo

    return exercise.to_output([], GitAutograderStatus.SUCCESSFUL)
```

Refer to existing `verify.py` scripts to understand what are the available helper functions to streamline the grading. Open an issue if there is something that is not yet supported or if you have a question.

Some examples of verifications:

- [branch-bender](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/branch_bender/verify.py)
- [conflict-mediator](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/conflict_mediator/verify.py)

### Verification conventions

1. Store the comments of the verification as constants so that they can be imported and used reliably in unit tests
2. For any remote behavior to verify, provide a mock to substitute the behavior in the unit tests

### Testing verification

To test the verification, we rely on [`repo-smith`](https://github.com/git-mastery/repo-smith) to simulate exercise states and write unit tests to verify the verification script's behavior. You don't need to simulate the entire flow, just the end states that you require for your verification script.

Refer to existing `test_verify.py` to see examples of unit testing the verification script.

You can run the unit tests of your exercise via:

```bash
./test.sh <your exercise folder>
```

## Submitting the exercise for review

Create a pull request from your fork using the provided pull request template.

Fill in all of the details necessary.

## Example

1. Exercise discussion: <https://github.com/git-mastery/exercises/issues/24>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Exercise PR: <https://github.com/git-mastery/exercises/pull/26>
