# Getting Started with Python with VS Code

## General

- Create a GitHub repository
  - for simplicity, the repository name will be the project directory name
  - you may use the the `gitignore` file here as your initial `.gitignore` file in the repository
  - you may also generate your `.gitignore` file from <https://gitignore.io/>
- Clone GitHub repository to the project's parent directory
- Confirm Python 3 has already been installed
  - `python3 --version`
- Go to the project directory

### Plain Pip

- Create a virtual environment in the project directory
  - `python3 -m venv venv`
- Activate the virtual environment, `venv`
  - `source venv/bin/activate`
- All Python packages installed from this point onwards will be available in this virtual environment only
- Install wheel (reference implementation of the Python wheel packaging standard)
  - `pip install wheel`
- Install pylint code analyser
  - `pip install pylint`
- Install flake8 code analyser
  - `pip install flake8`
- Install mypy static type checker
  - `pip install mypy`
- Install pydocstyle static analyser for Python docstrings
  - `pip install pydocstyle`
- Install Black code formatter
  - `pip install black`
- Install pytest for unit testing
  - `pip install pytest`
- Install Coverage.py for code coverage
  - `pip install coverage`
- Notes on requirements file
  - generating a `requirements.txt` file listing the installed dependencies
    - `pip freeze > requirements.txt`
  - separating deployment requirements from development requirements
    - create `requirements.txt` excluding all development packages
    - create `requirements_dev.txt` containing only development packages (such as those above)
      - add the line `-r requirements.txt` to the top of this file
  - creating a locked down version of production requirements
    - change all version specifiers to `==`
  - installing packages from requirements file
    - `pip install -r <requirements_file>`

### Pipenv

- Install Pipx
  - `python3 -m pip install --user pipx`
- Ensure directories necessary for Pipx operation are in your PATH environment variable
  - `python3 -m pipx ensurepath`
- Install Pipenv
  - `pipx install pipenv`
- Create an empty `.venv` directory under the project directory
  - this is to install packages into the project directory rather than in `~/.local/share/virtualenvs`
- Install development packages
  - `pipenv install pylint flake8 mypy pydocstyle black pytest coverage --dev`
- Activate the virtual environment
  - `pipenv shell`
- See also [README-pipenv.md](README-pipenv.md)

## Visual Studio Code (VS Code)

- Launch VS Code
- Install the Python extension (by Microsoft), if not already installed
- Open the project folder (directory)
- Select the `Python 3.x.x ('venv': venv)` interpreter
  - Command Palette: `python interpreter`
- Enable linting
  - Settings: `linting enabled`
    - 'Python > Linting: Enabled': check
- Enable pylint
  - Settings: `pylint enabled`
    - 'Python > Linting: Pylint Enabled': check
- Enable all pylint checkers
  - by default VS Code disables all Refactor, Convention, and most Warning messages
  - Settings: `pylintUseMinimalCheckers`
    - 'Pylint Use Minimal Checkers': uncheck
- Enable mypy
  - Settings: `mypy enabled`
    - 'Python > Linting: Mypy Enabled': check
- Enable pydocstyle
  - Settings: `pydocstyle`
    - 'Python › Linting: Pydocstyle Enabled': check
- Enable Black as the code formatter
  - Settings: `python formatting provider`
    - 'Python > Formatting: Provider': 'black'
- Enable Black to format code on save
  - Settings: `format on save`
- Set editor ruler and wrapping
  - the Black formatter defines an 88-character line length
  - Settings: `ruler`
  - under 'Editor: Rulers', click 'Edit in settings.json'
    - this opens the `settings.json` file
  - add the following right before the close curly brace (`}`) at the bottom of the file
    - `"editor.rulers": [88]`
  - ensure the line above it ends with a comma (`,`), e.g.,
    - `"editor.fontFamily": "..., 'Droid Sans Fallback'",`
  - Settings: `wrap`
    - 'Editor: Word Wrap': 'wordWrapColumn'
    - 'Editor: Word Wrap Column': '88'

## Miscellaneous Notes

- See <https://docs.python-guide.org/writing/structure/#sample-repository> for a recommended repository structure
- VS Code configuration (including extensions) are located in the following directories
  - `~/.vscode`
  - `~/.config/Code`

## Troubleshooting

**Linting with pylint disabled**

- Symptoms
  - pylint messages do not appear under the PROBLEMS tab
  - pylint messages are displayed when pylint is executed via command line
  - turning on Python linting via Command Palette has no effect - reverts to 'off'
  - selecting pylint as the linter via the Command Palette has no effect - reverts to 'Disable Linting' (if no other linters are enabled)
- Fix (workaround)
  - `settings.json` in both `~/.config/Code/User` and `<project_dir>/.vscode`, remove the following:
    - `"python.jediEnabled": false`
    - `"python.languageServer": "Microsoft"`
  - do _not_ re-enable the Microsoft language server when prompted

**Code Lens with pytest causes code to jump about**

- Symptoms
  - with Code Lens enabled and test discovery done, editing test code causes the 'Run Test' and 'Debug Test' Code Lens to appear and disappear
  - Code Lens appears when code is syntactically correct, and disappears when not
- Fix (workaround)
  - Settings:
    - 'Editor: Code Lens': uncheck
    - you may leave 'Python › Testing: Auto Test Discover On Save Enabled' checked
  - run tests via 'Tests' on the Activity Bar, or via command line terminal
