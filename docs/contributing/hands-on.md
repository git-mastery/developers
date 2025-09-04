---
title: Hands-on
parent: Contributing
nav_order: 1
---

1. TOC
{:toc}

## Before contributing

If you are proposing a new hands-on (i.e., not implementing an [already approved hands-on proposal](https://github.com/git-mastery/exercises/issues?q=is%3Aissue%20state%3Aopen%20label%3A%22hands-on%20discussion%22%20label%3A%22help%20wanted%22)) make sure that you have done the following:

- [ ] Create an [hands-on discussion](https://github.com/git-mastery/exercises/issues/new?template=hands_on_discussion.yaml)
- [ ] Obtain approval on the hands-on
- [ ] File a [remote repository request](https://github.com/git-mastery/exercises/issues/new?template=request_exercise_repository.yaml)

## Create a new hands-on

Use the provided `new.sh` script to generate the scaffolding for a new exercise:

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

## Download setup

Hands-on downloads are all single-file stored in `hands_on/`. The `<hands on name>.py` contains the instructions to setup the local repository.

{: .reference }

For more information about how Git-Mastery downloads hands-on, refer to the [Download Workflow](/developers/docs/architecture/download-workflow)

The default download script comes with a helper function to `run_command` to run local command.

These are some references for download setups for other exercises:

- [init-repo](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/init_repo.py)
- [add-files](https://raw.githubusercontent.com/git-mastery/exercises/refs/heads/main/hands_on/add_files.py)

### Download conventions

1. Any operations should use OS agnostic options (e.g. opting to use `shutil.rmtree` over `run_command(["rm"])`)
2. For any commands that require `gh` (Github CLI), make sure that the `__requires_github__` variable in the download setup is set to `True` so that the app will automatically check that the student has correctly setup Github CLI

### Testing downloads

To test that your download script works, we have provided a script to simulate the download process in this repository for you to verify.

```bash
./test-download.sh <your hands-on name>
```

You can find the downloaded repository under the `test-downloads/` folder.

## Submitting the hands-on for review

Create a pull request from your fork using the provided pull request template.

Fill in all of the details necessary.

## Example

1. Hands-on discussion: <https://github.com/git-mastery/exercises/issues/44>
2. Remote repository request: <https://github.com/git-mastery/exercises/issues/25>
3. Hands-on PR: <https://github.com/git-mastery/exercises/pull/45>

