---
date:
    created: 2025-09-11
categories:
    - Python
tags:
    - uv
---
# Python Package and Project Manager Tool – **uv**

**uv** is a single tool that replaces `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.

I’ve mostly used `pip`, `pipenv`, and `virtualenv` in the past, but **uv** combines most of these tasks into one tool. It’s faster, easier to learn, and highly efficient.

You can find installation instructions and documentation here: [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)

---

## Why uv?

* **uv** is a Python package and project manager that integrates multiple functionalities into one tool.
* It enables **fast dependency installation**, **virtual environment management**, **Python version management**, and **project initialization**, all designed to boost productivity.
* **uv** can build and publish Python packages to repositories like **PyPI**, streamlining the process from development to distribution.
* It automatically creates and manages **virtual environments**, ensuring clean and isolated project dependencies.

---

## Usage

### Installation

You can install uv from the command line:

* On **Windows**, use PowerShell.
* On **macOS/Linux**, use `curl`.
* You can also install it using **pipx**:

```bash
pipx install uv
```

Refer to the [official documentation](https://docs.astral.sh/uv/) for detailed installation options.

---

### Verify installation

```bash
uv --version
```

### Upgrade to the latest version

```bash
uv self update
# or, if installed via pipx
pipx upgrade uv
```

### Install multiple Python versions

```bash
uv python install 3.10 3.11 3.12
```

### Create a virtual environment with a specific version

```bash
uv venv --python 3.12.0
```

### Pin a specific Python version in the current directory

```bash
uv python pin 3.11
```

### View available and installed Python versions

```bash
uv python list
```

### Remove the uv, uvx, and uvw binaries
For Windows,
```bash
rm $HOME\.local\bin\uv.exe
rm $HOME\.local\bin\uvx.exe
rm $HOME\.local\bin\uvw.exe
```
For macOS,
```bash
rm ~/.local/bin/uv ~/.local/bin/uvx
```
---
