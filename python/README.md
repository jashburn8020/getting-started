# Getting Started with Python with VS Code

## General

- Create a GitHub repository.
  - For simplicity, the repository name will be the project directory name.
  - You may use the the `gitignore` file here as your initial `.gitignore` file in the repository.
  - You may also generate your `.gitignore` file from <https://gitignore.io/>.
- Clone GitHub repository to the project's parent directory.
- Confirm Python 3 has already been installed:
  - `python3 --version`
- Go to the project directory.

### Plain Pip

- Create a virtual environment in the project directory:
  - `python3 -m venv venv`
- Activate the virtual environment, `venv`:
  - `source venv/bin/activate`
- All Python packages installed from this point onwards will be available in this virtual environment only.
- Install Pylint code analyser:
  - `pip install pylint`
- Install Ruff code linter and formatter:
  - `pip install ruff`
- Install Mypy static type checker:
  - `pip install mypy`
- Install Pytest for unit testing:
  - `pip install pytest`
- Install Coverage.py for code coverage:
  - `pip install coverage`

#### Requirements file

- To generate a `requirements.txt` file listing the installed dependencies:
  - `pip freeze > requirements.txt`
- To separate deployment requirements from development requirements:
  - Create `requirements.txt` excluding all development packages.
  - Create `requirements_dev.txt` containing only development packages (such as those above).
    - Add the line `-r requirements.txt` to the top of this file.
- To create a locked down version of production requirements:
  - Change all version specifiers to `==`.
- To install packages from requirements file:
  - `pip install -r <requirements_file>`

### Pipenv

- Install Pipx:
  - `python3 -m pip install --user pipx`
- Ensure directories necessary for Pipx operation are in your PATH environment variable:
  - `python3 -m pipx ensurepath`
- Install Pipenv:
  - `pipx install pipenv`
- Create an empty `.venv` directory under the project directory.
  - This is to install packages into the project directory rather than in `~/.local/share/virtualenvs`.
- Install development packages:
  - `pipenv install pylint ruff mypy pytest coverage --dev`
- Activate the virtual environment:
  - `pipenv shell`
- See also [README-pipenv.md](README-pipenv.md).

## Visual Studio Code (VS Code)

- Launch VS Code.
- Install extensions:
  - Python (by Microsoft, if not already installed)
  - Pylint
  - Ruff
  - Mypy (it seems Matan Gover's extension works better than Microsoft's)
  - autoDocstring
- Open the project folder (directory).
- (Optional) Save as a workspace (if you'll be working on multiple projects with different setup requirements) in the project folder.
- Select the `Python 3.x.x ('venv': venv)` interpreter:
  - Command Palette: `python interpreter`
- Enable linters:
  - Linters, if installed, are enabled by default.
  - Disable a linter by disabling the extension, which can be done per workspace.
  - See also: <https://code.visualstudio.com/docs/python/linting>
- Enable all rules for Ruff:
  - Settings: `ruff select`
    - 'Ruff > Lint: Select': ALL
- Note: If you have legitimate reasons to need to use `print` statements:
  - Settings: `ruff lint ignore`
    - 'Ruff > Lint: Ignore': T201
- Enable type checking:
  - Settings: `mypy`
    - 'Mypy: Enabled': check
    - 'Mypy: Run Using Active Interpreter': check
  - Settings: `python type checking`
    - 'Python > Analysis: Type Checking Mode': strict
- Enable Pytest:
  - Settings: `pytest`
    - 'Python > Testing: Pytest Enabled': check
- Set editor ruler and wrapping:
  - The Ruff/Black formatter defines an 88-character line length.
  - Settings: `ruler`
    - 'Editor: Rulers', click 'Edit in settings.json'
      - This opens the `settings.json` file.
    - Add the following:
      - `"editor.rulers": [88]`
  - Settings: `wrap`
    - 'Editor: Word Wrap': 'wordWrapColumn'
    - 'Editor: Word Wrap Column': '88'

## Miscellaneous Notes

- See <https://docs.python-guide.org/writing/structure/#sample-repository> for a recommended repository structure
- VS Code configuration (including extensions) are located in the following directories
  - `~/.vscode`
  - `~/.config/Code`

## Troubleshooting

**Code Lens with pytest causes code to jump about**

- Symptoms
  - with Code Lens enabled and test discovery done, editing test code causes the 'Run Test' and 'Debug Test' Code Lens to appear and disappear
  - Code Lens appears when code is syntactically correct, and disappears when not
- Fix (workaround)
  - Settings:
    - 'Editor: Code Lens': uncheck
    - you may leave 'Python › Testing: Auto Test Discover On Save Enabled' checked
  - run tests via 'Tests' on the Activity Bar, or via command line terminal
