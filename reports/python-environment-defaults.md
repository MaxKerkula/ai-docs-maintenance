# Checked Python environment defaults

Test date: September 7, 2026.

The main checks used Windows PowerShell, CPython 3.14.2, Poetry 2.4.3, uv 0.12.10, and pytest 9.1.1. Tool installations, project environments, and configuration/cache directories were separate from existing projects. The investigation and command execution were done by Max Kerkula's AI coding assistant.

## Observed results

| Operation | Result |
| --- | --- |
| Poetry: `add --dev pytest` | Exit 0. The generated project contains pytest in its `dev` dependency group. |
| Poetry: `shell`, with the standard installation | Exit 1. The message says the command is not installed by default since Poetry 2.0.0. |
| Poetry: `run python -m pytest --version` | Exit 0; pytest 9.1.1. |
| Poetry: `sync --only main`, then the project interpreter's `-m pytest --version` | Sync exits 0 and removes pytest. The interpreter check exits 1 with `No module named pytest`. |
| Poetry: `install`, then the same interpreter check | Install exits 0 and restores pytest. The interpreter check exits 0; pytest 9.1.1. |
| uv: `add --dev pytest` | Exit 0. The generated project contains pytest in its `dev` dependency group. |
| uv: `sync --no-dev`, then the project interpreter's `-m pytest --version` | Sync exits 0 and removes pytest. The interpreter check exits 1 with `No module named pytest`. |
| uv: `sync`, then the same interpreter check | Sync exits 0 and restores pytest. The interpreter check exits 0; pytest 9.1.1. |
| uv: `run python -m pytest --version` | Exit 0; pytest 9.1.1. |

The interpreter checks used each project's `.venv/Scripts/python.exe` directly. This avoids mistaking a globally installed test runner for a project dependency. Each exclusion check preceded the corresponding default installation check, so existing pytest files could not explain the successful result.

A separate Poetry 1.8.5 installation on CPython 3.13.12 accepted `shell --help` with exit 0. This proves that the old command is registered. It does not prove an interactive shell session was tested. The guide's shell instruction has a version boundary; it was not always invalid.

## Proposed tutorial corrections

The Poetry and uv sections of [Modern Python Environments](https://testdriven.io/blog/python-environments/) describe development dependencies as excluded by default. The checked default installations include them. Poetry installs all non-optional groups; uv includes its `dev` group by default unless configuration changes that selection.

The Poetry section also needs an activation update for a standard Poetry 2.x installation. The existing `poetry run` approach works without adding a shell plugin. Activation through `poetry env activate`, or installing the optional shell plugin, are separate documented choices.

The `--dev` option itself works in Poetry 2.4.3. It must not be reported as a removed option.

For runtime-only environments, the tested commands are `poetry sync --only main` and `uv sync --no-dev`. These commands remove development tools from the selected environment. Keep a separate development environment when those tools are still needed.

## Scope and limits

These are dependency-selection and command-availability checks, not a complete rerun of the tutorial. The Flask example, interactive activation, Linux, and macOS were not tested in this investigation. The Poetry 1.8.5 check is limited to command help. No application code was changed, and no project-wide complexity or mutation audit is claimed.

The local raw logs include machine paths and generated author metadata. They are not published with this report. No payment identifier, customer data, or credentials are included here.

## Primary references

- [Poetry command reference](https://python-poetry.org/docs/cli/)
- [Poetry dependency groups and installation defaults](https://python-poetry.org/docs/managing-dependencies/)
- [Poetry environment activation](https://python-poetry.org/docs/managing-environments/)
- [Poetry 1.8 command reference](https://python-poetry.org/docs/1.8/cli/)
- [uv development dependencies and default groups](https://docs.astral.sh/uv/concepts/projects/dependencies/)
