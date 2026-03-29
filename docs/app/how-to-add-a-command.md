---
title: How to add a command
parent: App
nav_order: 1
---

# How to add a command

This guide walks through adding a new top-level command to the `gitmastery` CLI. The CLI uses [Click](https://click.palletsprojects.com/), a Python library for building command-line interfaces.

We'll use a simple `greet` command as an example throughout.

---

## 1. Create the command file

Create a new file under `app/app/commands/`:

```
app/app/commands/greet.py
```

Every command is a Python function decorated with `@click.command()`. Use the shared output helpers from `app.utils.click` instead of `print()`.

```python
# app/app/commands/greet.py
import click

from app.utils.click import info, success


@click.command()
@click.argument("name")
def greet(name: str) -> None:
    """
    Greets the user by name.
    """
    info(f"Hello, {name}!")
    success("Greeting complete.")
```

### Output helpers

| Helper | When to use |
|---|---|
| `info(msg)` | Normal status messages |
| `success(msg)` | Command completed successfully |
| `warn(msg)` | Non-fatal issues or warnings |
| `error(msg)` | Fatal issues — exits immediately |

---

## 2. Register the command in `cli.py`

Open `app/app/cli.py` and add two things:

1. Import your command at the top.
2. Add it to the `commands` list in `start()`.

```python
# app/app/cli.py

# 1. Import
from app.commands.greet import greet   # add this

# 2. Register
def start() -> None:
    commands = [check, download, greet, progress, setup, verify, version]  # add greet
    for command in commands:
        cli.add_command(command)
    cli(obj={})
```

---

## 3. Verify it works locally

Run the CLI directly from source to confirm the command appears and works:

```bash
uv run python main.py greet Alice
```

Expected output:

```
 INFO  Hello, Alice!
 SUCCESS  Greeting complete.
```

Also check it shows in `--help`:

```bash
uv run python main.py --help
```

---

## 4. Add an E2E test

Every new command needs an E2E test. Create a new file under `tests/e2e/commands/`:

```
tests/e2e/commands/test_greet.py
```

```python
# tests/e2e/commands/test_greet.py
from pathlib import Path

from ..runner import BinaryRunner


def test_greet(runner: BinaryRunner, gitmastery_root: Path) -> None:
    """greet prints the expected message."""
    res = runner.run(["greet", "Alice"], cwd=gitmastery_root)
    res.assert_success()
    res.assert_stdout_contains("Hello, Alice!")
```

### `RunResult` assertion methods

| Method | Description |
|---|---|
| `.assert_success()` | Asserts exit code is 0 |
| `.assert_stdout_contains(text)` | Asserts stdout contains an exact substring |
| `.assert_stdout_matches(pattern)` | Asserts stdout matches a regex pattern |

---

## 5. Run the E2E tests

Build the binary first, then run the suite:

```bash
# Build
uv run pyinstaller --onefile main.py --name gitmastery

# Set binary path (Unix)
export GITMASTERY_BINARY="$PWD/dist/gitmastery"

# Set binary path (Windows, PowerShell)
$env:GITMASTERY_BINARY = "$PWD\dist\gitmastery.exe"

# Run only your new test
uv run pytest tests/e2e/commands/test_greet.py -v

# Run the full E2E suite
uv run pytest tests/e2e/ -v
```

All tests should pass before opening a pull request.

---

## Command group (optional)

If you want to add a command that has subcommands (like `gitmastery progress show`), use `@click.group()` and register subcommands with `.add_command()`.

```python
# app/app/commands/greet.py
import click

from app.utils.click import info


@click.group()
def greet() -> None:
    """Greet people in different ways."""
    pass


@click.command()
@click.argument("name")
def hello(name: str) -> None:
    """Say hello."""
    info(f"Hello, {name}!")


@click.command()
@click.argument("name")
def goodbye(name: str) -> None:
    """Say goodbye."""
    info(f"Goodbye, {name}!")


greet.add_command(hello)
greet.add_command(goodbye)
```

Register `greet` the same way in `cli.py`. The user then runs:

```bash
gitmastery greet hello Alice
gitmastery greet goodbye Alice
```

---

## Checklist

Before opening a pull request for a new command:

- [ ] Command file created under `app/app/commands/`
- [ ] Command registered in `app/app/cli.py`
- [ ] Command works locally with `uv run python main.py`
- [ ] E2E test file created under `tests/e2e/commands/`
- [ ] E2E tests pass with the built binary

{: .reference }

See [E2E testing flow](/developers/docs/app/e2e-testing-flow) for a full explanation of the test infrastructure and how to write more complex tests.
