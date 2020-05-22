# Getting Started with Python with VS Code

- Create a GitHub repository
  - for simplicity, the repository name will be the project directory name
  - you may use the the `gitignore` file here as your initial `.gitignore` file in the repository
- Clone GitHub repository to the project's parent directory
- Confirm Python 3 has already been installed
  - `python3 --version`
- Go to the project directory
- Create a virtual environment in the project directory
  - `python3 -m venv venv`
- Activate the virtual environment, `venv`
  - `source venv/bin/activate`
- All Python packages installed from this point onwards will be available in this virtual environment only
- Install wheel (reference implementation of the Python wheel packaging standard)
  - `pip install wheel`
- Install pylint code analysis (checker)
  - `pip install pylint`
- Install Black code formatter
  - `pip install black`
- Install pytest for unit testing
  - `pip install pytest`
- Install Coverage.py for code coverage
  - `pip install coverage`

## Visual Studio Code (VS Code)

- Launch VS Code
- Install the Python extension (by Microsoft), if not already installed
- Open the project folder (directory)
- Select the 'Python 3.x.x ('venv': venv)' interpreter
  - Command Palette: 'python interpreter'
- Select pylint as the linter to use
  - Command Palette: 'linter'
- Enable all pylint checkers
  - by default VS Code disables all Refactor, Convention, and most Warning messages
  - Settings: 'pylintUseMinimalCheckers'
    - 'Pylint Use Minimal Checkers': uncheck
- Enable Black as the code formatter
  - Settings: 'python formatting provider'
    - 'Python > Formatting: Provider': 'black'
- Enable Black to format code on save
  - Settings: 'format on save'
- Set editor ruler and wrapping
  - the Black formatter defines an 88-character line length
  - Settings: 'ruler'
  - under 'Editor: Rulers', click 'Edit in settings.json'
    - this opens the `settings.json` file
  - add the following right before the close curly brace (`}`) at the bottom of the file
    - `"editor.rulers": [88]`
  - ensure the line above it ends with a comma (`,`), e.g.,
    - `"editor.fontFamily": "..., 'Droid Sans Fallback'",`
  - Settings: 'wrap'
    - 'Editor: Word Wrap': 'wordWrapColumn'
    - 'Editor: Word Wrap Column': '88'

## Miscellaneous Notes

- See https://docs.python-guide.org/writing/structure/#sample-repository for a recommended repository structure
