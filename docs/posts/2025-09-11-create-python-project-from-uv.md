---
date:
    created: 2025-09-11
categories:
    - Python
tags:
    - uv
---

# Create a Python Project Using uv and Manage Dependencies

## Create a Python Project

```bash
uv init pyprojects
```

This command automatically creates the following project structure inside the `pyprojects` folder:

```
.git
.gitignore
.python-version
main.py
pyproject.toml
README.md
```

## `pyproject.toml` Content

```toml
[project]
name = "pyprojects"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
```

## Run the Entry Script

```bash
uv run main.py
```

## Install Dependencies

Add the latest `requests` package to `pyproject.toml`:

```bash
uv add requests
```

This updates the file as follows:

```toml
dependencies = [
    "requests>=2.32.5",
]
```

Upgrade a package:

```bash
uv add --upgrade requests
```

Remove a package:

```bash
uv remove requests
```

## Add Development Dependencies

```bash
uv add --dev pytest
```

This creates a `[dependency-groups]` section in `pyproject.toml`:

```toml
[dependency-groups]
dev = [
    "pytest>=8.4.2",
]
```

## Add Dependencies from `requirements.txt`

```bash
uv add -r requirements.txt
```

## List Packages in the Virtual Environment

```bash
uv pip list
```

## Lock and Sync

Generate a lock file:

```bash
uv lock
```

Synchronize the environment with the lock file:

```bash
uv sync
```

---
