---
title: E2E testing flow
parent: App
nav_order: 4
---

# E2E testing flow

The `app` repository has an end-to-end test suite that runs the compiled `gitmastery` binary against real CLI commands. Tests assert on exit codes, stdout content, and side-effects such as file creation.

## Why E2E tests

Unit tests cover individual functions. E2E tests cover the full user-facing CLI path: argument parsing, command dispatch, file I/O, and cross-platform packaging behavior. Regressions caught only at this level include platform-specific path handling, binary encoding, and REPL dispatch.

## Test structure

```
tests/e2e/
├── conftest.py          # shared pytest fixtures
├── constants.py         # constants for test configuration
├── runner.py            # executes the built binary and returns RunResult
├── result.py            # helpers for assertions on exit code and stdout
├── utils.py             # helper utility functions
└── commands/            # tests for commands
    └── test_*.py
```

## Key components

### `BinaryRunner`

`BinaryRunner` wraps `subprocess.run` against the built binary. It is instantiated once per test session from the `GITMASTERY_BINARY` environment variable, falling back to `dist/gitmastery` (or `dist/gitmastery.exe` on Windows) if the variable is not set.

```python
runner.run(["check", "git"])
runner.run(["setup"], cwd=work_dir, stdin_text="\n")
runner.run(["progress", "sync", "off"], cwd=gitmastery_root, stdin_text="y\n")
```

### `RunResult`

Every `runner.run(...)` call returns a `RunResult` with chainable assertion helpers:

| Method                            | Description                               |
| --------------------------------- | ----------------------------------------- |
| `.assert_success()`               | Assert exit code is 0                     |
| `.assert_stdout_contains(text)`   | Assert stdout contains an exact substring |
| `.assert_stdout_matches(pattern)` | Assert stdout matches a regex pattern     |

```python
res = runner.run(["version"])
res.assert_success()
res.assert_stdout_contains("Git-Mastery app is")
res.assert_stdout_matches(r"v\d+\.\d+\.\d+")
```

### Fixtures in `conftest.py`

| Fixture                   | Scope    | Description                                                      |
| ------------------------- | -------- | ---------------------------------------------------------------- |
| `runner`                  | session  | Single `BinaryRunner` shared across all tests                    |
| `gitmastery_root`         | session  | Runs `gitmastery setup`, yields the root path, cleans up on exit |
| `setup_gitmastery_root`   | function | Same as above but for `test_setup.py` only                       |
| `downloaded_exercise_dir` | session  | Downloads exercise once, returns its path                        |
| `downloaded_hands_on_dir` | session  | Downloads hands-on once, returns its path                        |
| `verified_exercise_dir`   | session  | Runs `verify` on the downloaded exercise, returns its path       |

Session-scoped fixtures run once for the entire test run. This keeps the suite fast by reusing the same workspace across related tests.

## Running E2E tests locally

The tests run against the built binary, not the Python source directly. You must build first:

```bash
# Build the binary
uv run pyinstaller --onefile main.py --name gitmastery

# Point tests at the binary (Unix)
export GITMASTERY_BINARY="$PWD/dist/gitmastery"

# Point tests at the binary (Windows, PowerShell)
$env:GITMASTERY_BINARY = "$PWD\dist\gitmastery.exe"

# Run the suite
uv run pytest tests/e2e/ -v
```

{: .note }
E2E tests interact with GitHub (via `GH_TOKEN`) to download exercises and test progress sync. Ensure `gh auth status` succeeds and the `delete_repo` scope is granted before running locally.

```bash
gh auth refresh -s delete_repo
```

## CI workflows

Two GitHub Actions workflows run the E2E suite:

### `test.yml` — runs on every push to `main`

Steps:

1. Validate `GH_PAT` is present
2. Check out source
3. Install dependencies with `uv sync`
4. Build binary with `uv run pyinstaller --onefile main.py --name gitmastery`
5. Set `GITMASTERY_BINARY` path (platform-aware)
6. Configure Git user (`github-actions[bot]`)
7. Run `uv run pytest tests/e2e/ -v` with `GH_TOKEN=${{ secrets.GH_PAT }}`

### `test-pr.yml` — runs on pull requests

This workflow uses `pull_request_target` so it can access repository secrets. The `e2e-test` environment gate means a maintainer must approve the workflow run before secrets are available. This prevents untrusted PRs from accessing secrets while still allowing E2E tests to run in PRs from forks.

{: .note }

> Maintainers must do due diligence on PRs before approving the workflow run to ensure they do not contain malicious code that could access secrets. Look for unexpected changes to test files or the addition of new test files that could exfiltrate secrets. If in doubt, request changes and ask the contributor to provide evidence of what the test is doing and why it needs secrets access.

Steps are identical to `test.yml` except for the checkout step.

## When to add or update E2E tests

| Change                 | Action                                              |
| ---------------------- | --------------------------------------------------- |
| New top-level command  | Add `tests/e2e/commands/test_<command>.py`          |
| New subcommand or flag | Add a test case in the relevant test file           |
| Changed stdout message | Update `assert_stdout_contains` strings             |
| Changed exit behavior  | Update `assert_success()` or add failure assertions |
| Changed REPL behavior  | Update `test_repl.py`                               |

Prefer one test per distinct behavior. Keep assertions focused on user-visible output rather than internal state where possible.

## Writing a new E2E test

```python
from ..runner import BinaryRunner


def test_my_command(runner: BinaryRunner, gitmastery_root: Path) -> None:
    """my-command does X."""
    res = runner.run(["my-command"], cwd=gitmastery_root)
    res.assert_success()
    res.assert_stdout_contains("Expected output text")
```

Use the `gitmastery_root` fixture for commands that require a setup workspace. Use a fresh `setup_gitmastery_root` fixture only when the test must inspect the workspace in its initial pristine state.
