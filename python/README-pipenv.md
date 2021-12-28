# Pipenv

- Instal Pipx
  - `python3 -m pip install --user pipx`
  - upgrade Pipx with `python3 -m pip install --user -U pipx`
- Ensure directories necessary for Pipx operation are in your PATH environment variable
  - `python3 -m pipx ensurepath`
- Install Pipenv
  - `pipx install pipenv`
  - upgrade Pipenv with `pipx upgrade pipenv`
- Install packages
  - `pipenv install <package_name>`
    - installs package to `~/.local/share/virtualenvs/<dir_name>-<random>`
      - `<dir_name>` is the directory name in which `pipenv install` is run
      - e.g., running `pipenv install pycowsay` in the directory `py`
        - installs pycowsay to `~/.local/share/virtualenvs/py-3Qr_SmHK`
      - also places into the directory:
        - pip
        - setuptools
        - wheel
      - creates `Pipfile` and `Pipfile.lock` if they don't already exist, otherwise updates these files
  - to install a specified version of a package
    - `pipenv install <package_name>~=<major>.<minor>.<micro>`
      - `~=` is preferred over the `==` identifier as the latter prevents Pipenv from updating the packages
      - valid identifiers: [PEP-440](https://www.python.org/dev/peps/pep-0440/#version-specifiers)
  - to install a dev dependency, e.g., pytest
    - `pipenv install pytest --dev`
  - if a Pipfile already exists
    - `pipenv install`
  - note: running `pipenv install` in a subdirectory will install the package into the same `virtualenvs` location, but running it in a _sibling_ directory will install the package into a new location
- To install packages into the project directory rather than in `~/.local/share/virtualenvs`
  - create an empty `.venv` directory under the project directory before installing packages
  - alternative, set `export PIPENV_VENV_IN_PROJECT=1` in your `.bashrc`/`.zshrc` (or any shell configuration file)
- Install a path as editable
  - `pipenv install --dev -e .`
  - useful for the current working directory when working on packages
  - creates in Pipfile:

```toml
...
[dev-packages]
"e1839a8" = {path = ".", editable = true}
...
```

- Import from `requirements.txt`
  - running `pipenv install` will automatically import the contents of this file and create a Pipfile for you
  - if your requirements file has version numbers pinned, edit the new Pipfile to remove those, and let Pipenv keep track of pinning
- Pipfiles contain information for the dependencies of the project, and supersedes the `requirements.txt` file
  - keep both `Pipfile` and `Pipfile.lock` in version control
    - do not keep `Pipfile.lock` in version control if multiple versions of Python are being targeted
  - let users who clone the repository know the only thing required would be
    - installing Pipenv in the machine
    - typing `pipenv install`
      - `pipenv install` is fully compatible with [`pip install` syntax](https://pip.pypa.io/en/stable/user_guide/#installing-packages)
  - `Pipfile` uses the [TOML Spec](https://github.com/toml-lang/toml#user-content-spec)
- Create a `Pipfile.lock` file
  - `pipenv lock`
  - ensures repeatable and deterministic builds
  - to create `requirements.txt` instead:
    - `pipenv lock -r`, or
    - `pipenv run pip freeze`
- Pipenv upgrade workflow
  - find out what's changed upstream
    - `pipenv update --outdated`
  - upgrade packages
    - upgrade everything
      - `pipenv update`
    - upgrade packages one at a time
      - `pipenv update <package_name>`
- Activate the virtual environment
  - `pipenv shell`
