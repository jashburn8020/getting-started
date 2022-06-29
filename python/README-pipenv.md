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
    - `pipenv install --dev` (install all dependencies, including dev)
  - note: running `pipenv install` in a subdirectory will install the package into the same `virtualenvs` location, but running it in a _sibling_ directory will install the package into a new location
- To install packages into the project directory rather than in `~/.local/share/virtualenvs`
  - create an empty `.venv` directory under the project directory before installing packages
  - alternative, set `export PIPENV_VENV_IN_PROJECT=1` in your `.bashrc`/`.zshrc` (or any shell configuration file)
- Install a path as editable
  - `pipenv install --dev -e .`
  - useful for the current working directory when working on packages
  - creates in Pipfile:

```toml
[dev-packages]
"e1839a8" = {path = ".", editable = true}
```

- Import from `requirements.txt`
  - running `pipenv install` will automatically import the contents of this file and create a Pipfile for you
  - if your requirements file has version numbers pinned, edit the new Pipfile to remove those, and let Pipenv keep track of pinning
- Generate `requirements.txt`
  - default dependencies (for deployment) only:
    - `pipenv lock -r`, or
    - `pipenv run pip freeze`
  - include both the default and development dependencies:
    - `pipenv lock -r --dev`
  - development dependencies only:
    - `pipenv lock -r --dev-only`
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
- `Pipfile` and `Pipfile.lock` (for application development as opposed to library development)
  - `Pipfile`
    - define dependencies and where to get them
    - use this file to update the set of concrete dependencies in `Pipfile.lock`
      - a convenience for you to create that lock-file and allows you to still remain somewhat vague about the exact version of a dependency to be used
  - `Pipfile.lock`
    - defines a specific idempotent environment that is known to work for your project
    - your source of truth
- List installed dependencies
  - `pipenv graph`
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
- Deploy to production
  - if you use docker and you don't want a virtualenv to be created
    - `pipenv install --system --deploy`
  - install packages exactly as specified in `Pipfile.lock`
    - `pipenv sync`

## Example

- In development: development application with the version of `requests` that is available at the time
  - install `requests` to develop application: `pipenv install requests`
    - `requests` package v2.25.0 installed at this time
    - `Pipfile`: `requests = "*"`
    - `Pipfile.lock`: `"version": "==2.25.0"`
  - add both `Pipfile` and `Pipfile.lock` to source control
- `requests` v2.26.0 is released
- In production: deploy application
  - pull application, `Pipfile`, and `Pipfile.lock` from source control into production
  - deploy application: `pipenv install --deploy`
    - deployment aborted: `Your Pipfile.lock (7f8019) is out of date. Expected: (fbd99e).`
  - 2 choices:
    1. install anyway, exactly as specified in `Pipfile.lock`: `pipenv sync`
    2. test application with `requests` v.2.26.0 in development (below) before (re)deploying it
- In development: update `requests`
  - check outdated packages: `pipenv update --outdated`
    - shows: `Package 'requests' out-of-date: '==2.25.0' installed, '==2.26.0' available.`
  - update `requests`: `pipenv update`
    - `Pipfile` unchanged: `requests = "*"`
    - `Pipfile.lock` updated: `"version": "==2.26.0"`
  - push `Pipfile.lock` to source control
- After testing application with updated `requests`, deploy to production
