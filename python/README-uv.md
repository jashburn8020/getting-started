# uv

- Install uv
  - See <https://docs.astral.sh/uv/getting-started/installation/>.

```bash
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.12.19 x86_64-unknown-linux-gnu
installing to /home/<name>/.local/bin
  uv
  uvx
everything's installed!
```

- Create a new project
  - Creates `pyproject.toml` and sets up Python.

```bash
$ uv init test-uv
Initialized project `test-uv` at `/home/<name>/python/test-uv`

$ cd test-uv

$ tree -a -L 1 .
.
├── .git
├── .gitignore
├── pyproject.toml
├── .python-version
├── README.md
├── src
├── uv.lock
└── .venv

$ cat pyproject.toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "<name>", email = "<email>" }
]
requires-python = ">=3.13"
dependencies = []

[project.scripts]
test-uv = "test_uv:main"

[build-system]
requires = ["uv_build>=0.12.19,<0.13.0"]
build-backend = "uv_build"
```

- `[project.scripts]` defines the entry point.
  - `test-uv = "test_uv:main"` declares a command called `test-uv` that invokes the `main` function in the `test_uv` module.
  - `test_uv` can be `test_uv.py` or a `test_uv` directory containing `__init__.py`.

```bash
$ tree -a -L 3 src
src
└── test_uv
    └── __init__.py

2 directories, 1 file

$ cat src/test_uv/__init__.py
def main() -> None:
    print("Hello from test-uv!")

$ uv run python -c "import shutil; print(shutil.which('test-uv'))"
/home/<name>/python/test-uv/.venv/bin/test-uv

$ cat .venv/bin/test-uv
#!/home/<name>/python/test-uv/.venv/bin/python
# -*- coding: utf-8 -*-
import sys
from test_uv import main
if __name__ == "__main__":
    if sys.argv[0].endswith("-script.pyw"):
        sys.argv[0] = sys.argv[0][:-11]
    elif sys.argv[0].endswith(".exe"):
        sys.argv[0] = sys.argv[0][:-4]
    sys.exit(main())

$ uv run test-uv
Hello from test-uv!
```

- Add top level (production) packages
  - Updates `pyproject.toml`, creates `.venv` directory, and creates `uv.lock`.

```bash
$ uv add requests flask
Using CPython 3.13.5
Creating virtual environment at: .venv
Resolved 13 packages in 155ms
      Built test-uv @ file:///home/<name>/python/test-uv
Prepared 13 packages in 404ms
Installed 13 packages in 2ms
 + blinker==1.9.0
 + ...
 + werkzeug==3.1.9

$ cat pyproject.toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "<name>", email = "<email>" }
]
requires-python = ">=3.13"
dependencies = [
    "flask>=3.1.3",
    "requests>=2.34.2",
]

[project.scripts]
test-uv = "test_uv:main"

[build-system]
requires = ["uv_build>=0.12.19,<0.13.0"]
build-backend = "uv_build"
```

- Add development tools to the `dev` dependency group.
  - Dependency groups allow you to separate development, testing, or linting tools from your core production code.

```bash
$ uv add --group dev pytest ruff
Resolved 20 packages in 256ms
      Built test-uv @ file:///home/<name>/python/test-uv
Prepared 7 packages in 792ms
Uninstalled 1 package in 0.72ms
Installed 7 packages in 15ms
 + iniconfig==2.3.0
 + packaging==26.3
 + pluggy==1.6.0
 + pygments==2.21.0
 + pytest==9.1.1
 + ruff==0.16.9
 ~ test-uv==0.1.0 (from file:///home/<name>/python/test-uv)

$ cat pyproject.toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
authors = [
    { name = "<name>", email = "<email>" }
]
requires-python = ">=3.13"
dependencies = [
    "flask>=3.1.3",
    "requests>=2.34.2",
]

[project.scripts]
test-uv = "test_uv:main"

[build-system]
requires = ["uv_build>=0.12.19,<0.13.0"]
build-backend = "uv_build"

[dependency-groups]
dev = [
    "pytest>=9.1.1",
    "ruff>=0.16.9",
]
```

- To remove `flask` and upgrade `requests`:

```bash
# Removes flask AND automatically purges orphaned sub-dependencies (werkzeug, jinja2, etc.)
uv remove flask

# Upgrades requests and updates its transitive sub-dependencies
uv add "requests>=3.0"
```

- To remove packages from groups:

```bash
uv remove --group dev pytest
```

- Commit to Git:
  - `pyproject.toml` (human-readable top-level requirements)
  - `uv.lock` (machine-readable exact dependency graph)
  - source files
- To sync environment:
  - Local development (default):
    - `uv sync`
    - By default, this installs production dependencies plus the `dev` group
  - To include additional optional groups alongside `dev`:
    - `uv sync --group docs`
  - To include all defined dependency groups at once:
    - `uv sync --all-groups`
  - To explicitly exclude development tools (in production servers or Docker builds):
    - `uv sync --no-dev`

## Migrating an existing project from a standard `pip` setup to uv

- Navigate to your project root and initialize uv.
  - Creates a standard `pyproject.toml` file without altering your existing code.
  - The `--bare` flag tells uv to configure the project in your current directory rather than creating a new subdirectory or sample Python file.

```bash
cd /path/to/your/project
uv init --bare
```

- Instead of dumping your raw `requirements.txt` (which contains secondary dependencies pinned by `pip freeze`), identify your top-level dependencies and add them using `uv add`.

```bash
# Production dependencies
uv add requests flask pandas

# Development/testing dependencies
uv add --group dev pytest ruff mypy
```

- uv will automatically:
  - Write the top-level packages to `pyproject.toml`.
  - Resolve all transitive dependencies cleanly.
  - Generate a `uv.lock` file.
  - Create a fresh `.venv` virtual environment in the background.
- Test and run.
- Remove the old virtual environment and requirements file.
