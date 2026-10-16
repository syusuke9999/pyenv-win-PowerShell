# pyenv-win PowerShell

`pyenv-win PowerShell` is a PowerShell-oriented fork of [pyenv-win](https://github.com/pyenv-win/pyenv-win), the Windows port of [pyenv](https://github.com/pyenv/pyenv). It helps you install, select, and switch between multiple Python versions on Windows.

This repository contains:

- the batch entrypoint at `pyenv-win/bin/pyenv.bat`
- the PowerShell entrypoint at `pyenv-win/bin/pyenv.ps1`
- the installer script at `pyenv-win/install-pyenv-win.ps1`
- command implementations under `pyenv-win/libexec`
- automated tests under `tests`

## Contents

- [Introduction](#introduction)
- [pyenv](#pyenv)
- [Quick start](#quick-start)
- [Repository layout](#repository-layout)
- [pyenv-win commands](#pyenv-win-commands)
- [Installation](#installation)
- [Validate installation](#validate-installation)
- [Usage](#usage)
- [How to update pyenv](#how-to-update-pyenv)
- [Announcements](#announcements)
- [FAQ](#faq)
- [Changelog](#changelog)
- [How to contribute](#how-to-contribute)
- [Bug Tracker and Support](#bug-tracker-and-support)
- [License and Copyright](#license-and-copyright)

## Introduction

This fork keeps the upstream `pyenv-win` layout and command set, while also shipping a PowerShell wrapper for `pyenv shell`. In PowerShell and `pwsh`, `pyenv-win/bin/pyenv.ps1` updates `PYENV_VERSION` in the current session; other commands are forwarded to the batch implementation.

## pyenv

`pyenv` is a Python version manager. On Windows, this repository provides the files needed to install Python versions into `~/.pyenv/pyenv-win`, choose global or local versions, and resolve executables through shims.

## Quick start

1. Follow one of the installation methods in [docs/installation.md](./docs/installation.md).
2. Reopen your terminal.
3. Confirm the installation:

   ```powershell
   pyenv --version
   pyenv commands
   ```

4. List installable versions:

   ```powershell
   pyenv install -l
   ```

5. Install and activate a version:

   ```powershell
   pyenv install 3.11.3
   pyenv global 3.11.3
   python --version
   ```

## Repository layout

| Path | Purpose |
| --- | --- |
| `pyenv-win/bin` | User-facing launchers (`pyenv`, `pyenv.bat`, `pyenv.ps1`) |
| `pyenv-win/libexec` | Command implementations such as `install`, `latest`, `which`, and `whence` |
| `pyenv-win/install-pyenv-win.ps1` | Installer and updater script |
| `docs` | Installation, FAQ, and changelog documents |
| `tests` | Pytest suite for command behavior across shells and architectures |
| `.github/scripts` | CI scripts used by GitHub Actions |

## pyenv-win commands

Run `pyenv commands` to print the commands currently available from `pyenv-win/libexec`.

| Command | Purpose |
| --- | --- |
| `pyenv --version` | Show the installed pyenv-win version |
| `pyenv commands` | List available pyenv commands |
| `pyenv duplicate <source> <target>` | Duplicate an installed environment inside `versions/` |
| `pyenv exec <command> [args...]` | Run a command with the selected Python version at the front of `PATH` |
| `pyenv export <source> <path>` | Copy an installed environment to another directory |
| `pyenv global <version> [<version> ...]` | Set or show the global Python version list |
| `pyenv help` | Show general help |
| `pyenv install [-s] [-f] <version> [<version> ...]` | Install one or more Python versions |
| `pyenv latest [-k|--known] [-q|--quiet] <prefix>` | Resolve the latest installed or known version for a prefix |
| `pyenv local <version> [<version> ...]` | Set or show the local Python version list |
| `pyenv migrate <from> <to>` | Reinstall pip packages from one Python version into another |
| `pyenv rehash` | Rebuild shims after installing executables |
| `pyenv shell <version> [<version> ...]` | Set or show the shell-specific version list |
| `pyenv shims [--short]` | List generated shims |
| `pyenv uninstall [-f|--force] <version> [<version> ...]` | Remove one or more installed versions |
| `pyenv update [--ignore]` | Refresh the cached version database |
| `pyenv version` | Show the active version and where it was set |
| `pyenv version-name` / `pyenv vname` | Show the resolved active version name |
| `pyenv versions [--bare]` | List installed versions |
| `pyenv which <command>` | Show the full path to a resolved executable |
| `pyenv whence [--path] <command>` | Show which installed versions provide an executable |

## Installation

The repository includes installation instructions for these supported paths:

- [PowerShell](./docs/installation.md#powershell)
- [Git Commands](./docs/installation.md#git-commands)
- [Pyenv-win zip](./docs/installation.md#pyenv-win-zip)
- [Python pip](./docs/installation.md#python-pip)
- [Chocolatey](./docs/installation.md#chocolatey)
- [How to use 32-train](./docs/installation.md#how-to-use-32-train)

See [docs/installation.md](./docs/installation.md) for the full setup steps.

## Validate installation

1. Restart your terminal or IDE terminal.
2. Confirm that `pyenv` is on `PATH`:

   ```powershell
   pyenv --version
   pyenv help
   ```

3. Confirm that version discovery works:

   ```powershell
   pyenv install -l
   ```

4. If `pyenv` is not found, verify the environment variable and `PATH` settings below.

### Manually check the settings

Expected user environment variables:

| Variable | Value |
| --- | --- |
| `PYENV` | `%USERPROFILE%\.pyenv\pyenv-win\` |
| `PYENV_ROOT` | `%USERPROFILE%\.pyenv\pyenv-win\` |
| `PYENV_HOME` | `%USERPROFILE%\.pyenv\pyenv-win\` |

Expected `PATH` entries:

```text
%USERPROFILE%\.pyenv\pyenv-win\bin
%USERPROFILE%\.pyenv\pyenv-win\shims
```

If you are on Windows 10 or newer and `python` still resolves incorrectly, disable the built-in App Installer aliases for Python from **Manage App Execution Aliases**.

## Usage

- List installable versions: `pyenv install -l`
- Install one or more versions: `pyenv install 3.10.11 3.11.3`
- Set a default version for the machine user: `pyenv global 3.11.3`
- Set a directory-local version: `pyenv local 3.11.3`
- Set a session-only version: `pyenv shell 3.11.3`
- Show the active version and its origin: `pyenv version`
- Show the resolved version name only: `pyenv version-name`
- Rebuild shims after installing tools with `pip`: `pyenv rehash`
- Find the executable path that pyenv would run: `pyenv which python`
- See which installed versions provide a tool: `pyenv whence pip`

`global`, `local`, and `shell` all support multiple version arguments, and `versions` marks whichever entries are currently active.

## How to update pyenv

`pyenv update` refreshes the cached list of installable Python versions. To update the tool itself, use the same channel you used to install it:

- **pip**: `pip install --upgrade pyenv-win`
- **Git or zip checkout**: replace the checked-out files with a newer copy of this repository and rerun your setup steps if needed
- **PowerShell installer**: rerun `&"${env:PYENV_HOME}\install-pyenv-win.ps1"`

## Announcements

- On 64-bit Windows, an unsuffixed version such as `3.11.3` resolves to the native architecture, while `3.11.3-win32` selects the 32-bit build.
- For the legacy naming branch, see [How to use 32-train](./docs/installation.md#how-to-use-32-train).
- Python versions earlier than 2.4 are not supported.

## FAQ

See [docs/faq.md](./docs/faq.md).

## Changelog

See [docs/changelog.md](./docs/changelog.md).

## How to contribute

- Fork the repository and work in a branch.
- Keep changes focused and add or update tests when behavior changes.
- Install development dependencies from `requirements_dev.txt`.
- Run the same pytest command used by CI:

  ```bash
  PYTHONPATH=. python -m pytest -v -s --cache-clear --cov=pyenv-win tests
  ```

- The test suite exercises `cmd`, `powershell`, and `pwsh`, and it runs under both `AMD64` and `X86` fixture modes.

## Bug Tracker and Support

Please use the issue tracker for this repository: <https://github.com/syusuke9999/pyenv-win-PowerShell/issues>

## License and Copyright

This repository is licensed under the [MIT License](./LICENSE).
