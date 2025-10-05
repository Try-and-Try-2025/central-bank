
=========
Learn UV 
=========

:Version: 2025-10

.. admonition:: Objectives
    :class: important

    This article is intended to know all the basics command for uv, the modern Python package manager.
    It is not a complete documentation, but it will help you to get started with uv.
    You can find the complete documentation on the official website: https://uv.readthedocs

Basics Commands for uv for your daily work on Python projects
=============================================================

uv is a modern Python package manager that simplifies the management of Python environments and dependencies. It is designed to be user-friendly and efficient, making it easier for developers to work with Python projects.

1. Installation of uv (if not already installed)

.. tab:: Linux

    .. code-block:: bash

        curl -LsSf https://astral.sh/uv/install.sh | sh

.. tab:: Powershell

    .. code-block:: bash

        powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

2. Start a new project with uv

.. code-block:: bash

    uv init --package keyrus_tuto_uv

A note that the `uv init` command creates a new directory named `keyrus_tuto_uv` and initializes a new uv project in it. You can replace `keyrus_tuto_uv` with any name you prefer for your project.

Adding the `--package` option will allow you to directly create a Python package

3. Navigate to the project directory

.. code-block:: bash

    cd keyrus_tuto_uv

We can check the content of the project directory with the command: ``tree``. You will see the following structure:

.. code-block:: bash

    .
    ├── README.md
    ├── pyproject.toml
    └── src
        └── keyrus_tuto_uv
            └── __init__.py

    3 directories, 3 files

and if you look at the `pyproject.toml`, you already have a very good starting point:

.. code-block:: bash

   cat pyproject.toml

You should see:

.. code-block:: bash

   [project]
    name = "keyrus-tuto-uv"
    version = "0.1.0"
    description = "Add your description here"
    readme = "README.md"
    authors = [
        { name = "Cao Tri DO", email = "cao-tri.do@keyrus.com" }
    ]
    requires-python = ">=3.12"

    [project.scripts]
    keyrus-tuto-uv = "keyrus_tuto_uv:main"

    [build-system]
    requires = ["uv_build>=0.8.17,<0.9.0"]
    build-backend = "uv_build"

4. To create a virtual environment, you can use the following command:

.. code-block:: bash

    uv venv

5. To activate the virtual environment, use the command (the same as for venv).

.. code-block:: bash

    source .venv/bin/activate

.. note::
   Note that it is not necessary to activate the virtual environment to install packages with uv because each command starting with `uv` will automatically use the virtual environment if it exists.

6. To install a package (e.g. pandas and streamlit), use the command:

.. code-block:: bash

    uv add scikit-learn loguru

The packages will be both installed in the virtual environment and added to the `pyproject.toml` file. The `uv.lock` file will also be updated to reflect the installed packages and their versions.

And to add packages only for development (dev), you can create a group:

.. code-block:: bash

    uv add --group dev pytest ruff

and let's create a group for testing purpose

.. code-block:: bash

    uv add --group test pytest

We can check the packages installed in the virtual environment with the command:

.. code-block:: bash

    uv pip list

What is interesting with uv, is that it is possible to see the dependency tree of our installed packages. For this:

.. code-block:: bash

    uv tree

In our case, you will see the following output:

.. code-block:: bash

    keyrus-tuto-uv v0.1.0
    ├── pandas v2.3.0
    │   ├── numpy v2.3.1
    │   ├── python-dateutil v2.9.0.post0
    │   │   └── six v1.17.0
    │   ├── pytz v2025.2
    │   └── tzdata v2025.2
    └── streamlit v1.46.1
        ├── altair v5.5.0
        │   ├── jinja2 v3.1.6
        │   │   └── markupsafe v3.0.2
        │   ├── jsonschema v4.24.0
        │   │   ├── attrs v25.3.0
        │   │   ├── jsonschema-specifications v2025.4.1
        │   │   │   └── referencing v0.36.2
        │   │   │       ├── attrs v25.3.0
        │   │   │       ├── rpds-py v0.26.0
        │   │   │       └── typing-extensions v4.14.1
        │   │   ├── referencing v0.36.2 (*)
        │   │   └── rpds-py v0.26.0
        │   ├── narwhals v1.45.0
        │   ├── packaging v25.0
        │   └── typing-extensions v4.14.1
        ├── blinker v1.9.0
        ├── cachetools v6.1.0
        ├── click v8.2.1
        ├── gitpython v3.1.44
        │   └── gitdb v4.0.12
        │       └── smmap v5.0.2
        ├── numpy v2.3.1
        ├── packaging v25.0
        ├── pandas v2.3.0 (*)
        ├── pillow v11.3.0
        ├── protobuf v6.31.1
        ├── pyarrow v20.0.0
        ├── pydeck v0.9.1
        │   ├── jinja2 v3.1.6 (*)
        │   └── numpy v2.3.1
        ├── requests v2.32.4
        │   ├── certifi v2025.6.15
        │   ├── charset-normalizer v3.4.2
        │   ├── idna v3.10
        │   └── urllib3 v2.5.0
        ├── tenacity v9.1.2
        ├── toml v0.10.2
        ├── tornado v6.5.1
        ├── typing-extensions v4.14.1
        └── watchdog v6.0.0


7. To update the virtual environment with the latest versions of the packages, use the command:

.. code-block:: bash

    uv sync

.. note::
   A file uv.lock is created that contains the package on the system level

8. To remove a package (e.g. pandas), use the command:

.. code-block:: bash

    uv remove pandas

9. To run a Python script (e.g. `main.py`), use the command:

Let's create a `main.py` file in `src/main.py`:

.. code-block:: bash

    nano src/main.py

and let's add a simple script to train a ML Classifier on Iris Dataset

.. code-block:: python

    # main.py
    from loguru import logger
    from sklearn.datasets import load_iris
    from sklearn.model_selection import train_test_split
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.metrics import accuracy_score


    def main():
        logger.info("Loading the Iris dataset...")
        iris = load_iris()
        X, y = iris.data, iris.target

        logger.info("Splitting into train/test sets...")
        X_train, X_test, y_train, y_test = train_test_split(
            X, y, test_size=0.2, random_state=42
        )

        logger.info("Initializing RandomForest model...")
        model = RandomForestClassifier(n_estimators=100, random_state=42)

        logger.info("Training the model...")
        model.fit(X_train, y_train)

        logger.info("Evaluating the model...")
        y_pred = model.predict(X_test)
        acc = accuracy_score(y_test, y_pred)

        logger.success(f"Test accuracy: {acc:.4f}")


    if __name__ == "__main__":
        logger.add("train.log", rotation="1 MB", retention="7 days", level="INFO")
        logger.info("Starting training script.")
        main()
        logger.info("Training script finished.")

Then you can run the script:

.. code-block:: bash

    uv run src/main.py

10. To run a script with a specific package only for this run, use the command `uv run --with`:

For example, let's remove loguru

.. code-block:: bash

    uv remove loguru

if we run the code: `uv run src/main.py`, we will obtain an error:

.. code-block:: bash

    ctdo@ZL008776:~/tmp/keyrus_tuto_uv git:(HEAD)> uv run src/main.py
    Traceback (most recent call last):
    File "/home/ctdo/tmp/keyrus_tuto_uv/src/main.py", line 2, in <module>
        from loguru import logger
    ModuleNotFoundError: No module named 'loguru'

To run this script with loguru, you can use `uv run --with`

.. code-block:: bash

    uv run --with loguru src/main.py

This should work again !!

.. note::
   If you want to run with a group, for example, `test`, you can do

   .. code-block:: bash

      uv run --group pytest

11. Finally, to delete the environment, just delete the `.venv/` folder

.. tab:: Linux

    .. code-block:: bash

        rm -rf .venv

.. tab:: Powershell

    .. code-block:: bash

        Remove-Item .\.venv\

Advanced Commands
=================

1. To commit properly, use commitizen:

Let's add all our new file

.. code-block:: bash

   git add .

.. code-block:: bash

   uv run --with commitizen cz c

and let's put the commit message

.. code-block:: bash

   feat: initial commit

2. Build a package

.. code-block:: bash

    uv build

This command will create a `dist` directory containing the built package files.

2. Publish a package

.. code-block:: bash

    uv publish

This command will publish the package to the Python Package Index (PyPI) or any other specified repository.

3. To bump the version of your package (tag):

let's add this to your `pyproject.toml`

.. code-block:: bash

    [tool.commitizen]
    name = "cz_conventional_commits"
    version_provider = "pep621"
    tag_format = "v$version"
    version_files = [
    "pyproject.toml:^version =",
    "VERSION"
    ]
    bump_message = "chore(release): bump version $current_version → $new_version [skip ci]"
    update_changelog_on_bump = true
    changelog_incremental = true
    pre_bump_hooks = [
        "uv sync",
        "git add uv.lock"
    ]
    changelog_file = "CHANGELOG.md"
    changelog_title = "# Changelog \\n\\nAll notable changes to this project will be documented in this file.\n\nThe format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),\nand this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).\\n\\n## [Unreleased]\\n"
    changelog_message_template = """
    ## {{version}} - {{date}}

    {{#each commits}}
    - {{message}}
    {{/each}}

    """

and let's create a `VERSION` file with version 0.1.0:

.. code-block:: bash

   echo "0.1.0" >> VERSION

Then, you can use the command:

.. code-block:: bash

    uv run --with commitizen cz bump

4. To make a digest of your code (all your code in one .md file):

.. code-block:: bash

   uv run --with gitingest gitingest .

5. To run the ds-make squeleton from the DS team:

.. code-block:: bash

   uv run --with cookiecutter cookiecutter git@gitlab.com:keyrus-data/infra/cookiecutter-data-squeleton.git --directory="data-science-v2"

Link to our squeleton
=====================

And if you don't want to retain all of these commands, you can put everything in a Makefile:

1. Create a Makefile:

.. code-block:: bash

   nano Makefile

2. Add this template for Makefile

.. code-block:: bash

    # Source for a good Python Makefile: https://gist.github.com/genyrosk/2a6e893ee72fa2737a6df243f6520a6d

    #################################################################################
    # GLOBALS                                                                       #
    #################################################################################

    PROJECT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

    -include .env
    export

    #################################################################################
    # HELP (default) - shows all available make commands
    #################################################################################
    .PHONY: help .env
    help: .env
            @echo "Available commands:"
            @grep -E '^[a-zA-Z_-]+:.*?## ' Makefile | awk 'BEGIN {FS = ":.*?## "}; {printf "  \033[36m%-15s\033[0m %s\n", $$1, $$2}'

    .DEFAULT_GOAL := help

    #################################################################################
    # PROJECT RULES                                                                 #
    #################################################################################
    .PHONY: run_app run run_api

    run: ## Run the script demo
            @echo "$(ccso)--> Run the Script demo$(ccend)"
            uv run src/main.py

    #################################################################################
    # COMMANDS (INSTALL / SETUP)                                                    #
    #################################################################################
    .PHONY: dev-install prod-install

    dev-install: ## Install Python Dependencies (Dev)
            @echo "$(ccso)--> Install Python Dependencies (Dev)$(ccend)"
            uv sync --dev

    prod-install: ## Install Python Dependencies (Prod)
            @echo "$(ccso)--> Install Python Dependencies (Dev)$(ccend)"
            uv sync

    clean: ## Delete all compiled Python files
            @echo "$(ccso)--> Delete all compiled Python files$(ccend)"
            find . -type f -name "*.py[co]" -delete
            find . -type d -name "__pycache__" -delete
            find . -name '*.egg-info'  -type d -exec rm -rf {} +
            rm -rf .venv/
            rm -rf _artifact/ .devbox/ .pytest_cache/ .ruff_cache/ node_modules/
            rm .coverage package.json package-lock.json digest.txt zz_git-diff.txt

    uv-install: ## Install uv
            @echo "$(ccso)--> Install uv$(ccend)"
            curl -LsSf https://astral.sh/uv/install.sh | sh
            @echo "Note: restart your terminal or run: source ~/.bashrc or source ~/.zshrc"

    install: uv-install ## Install uv
            @echo "$(ccso)--> Initiating isolated shell dev environment$(ccend)"
            devbox shell

    #################################################################################
    # COMMANDS (CODE QUALITY & BUILD)                                               #
    #################################################################################
    .PHONY: qa_lines_count test pc

    qa_lines_count: ## Code quality: count nb lines in each *.py files
            @echo "$(ccso)--> Count the number of lines in each *.py script$(ccend)"
            @find ./src ./tests -name '*.py' -exec  wc -l {} \; | sort -n| awk \
                '{printf "%4s %s\n", $$1, $$2}{s+=$$0}END{print s}'
            @echo ''

    test: ## Run unit test and coverage
            @echo "$(ccso)--> Run unit test and coverage$(ccend)"
            uv run --group test pytest --cov src/$(PACKAGE_NAME) --cov-report=html:_artifact/coverage_re --cov-report=term-missing --cov-config=.coveragerc
            @echo "Note: you can open the HTML report with: open _artifact/coverage_re/index.html"
            @echo

    pc: ## Run pre-commit hooks
            @echo "$(ccso)--> Run pre-commit rules$(ccend)"
            @pre-commit run --all-files

    bump: ## Bump the version using commitizen
            @echo "$(ccso)--> Bump the version using commitizen$(ccend)"
            @uv run cz bump
            @echo "Note: don't forget to push the new tag to the remote repo with: git push --tags"

    commit: ## Commit your changes using commitizen
            @echo "$(ccso)--> Commit your changes using commitizen$(ccend)"
            @uv run cz commit

    #################################################################################
    # UTILITIES                                                                 #
    #################################################################################
    .PHONY: pipreqs generate_docs create-env-template

    ## Generate a requirements-tmp.txt file from the src/ folder
    pipreqs:
            @echo "$(ccso)--> Generate a requirements-tmp.txt file from the src/ folder$(ccend)"
            @uv run --with pipreqs pipreqs src/ --savepath requirements-tmp.txt
            @echo "List of the standard packages used in the code"
            @cat requirements-tmp.txt

    mr-desc: ## Get the git diff between main and dev branches and save it to a file
            git diff main dev > zz_git-diff.txt

    digest-code: ## Create a unique .md file from all the repo codes
            @echo "$(ccso)--> Create a unique .md file from all the repo codes$(ccend)"
            uv run --with gitingest gitingest .

    #################################################################################
    # Self Documenting Commands                                                     #
    #################################################################################
    .PHONY: help_setup

    ## Initial setup to follow
    help_setup:
            @echo "$(ccso)--> Initial setup to follow$(ccend)"
            @echo "1. Install Devbox, uv and docker"
            @echo "2. Start a new isolated shell using: devbox shell"
            @echo "3. Install all the required Dependencies using uv sync or make dev-install"
            @echo "4. Run the app using: make run (script) or make run_app (streamlit app) or make run_api (FastAPI server)"
            @echo "5. Alternatively, you can run all the application using docker-compose: docker-compose up --build or make start"
            @echo "6. (optional) clean your environment using: make clean"
            @echo


    #################################################################################
    # ENV CHECK & CONFIGURATION                                                     #
    #################################################################################

    # Colors for echos
    ccend=$(shell tput sgr0)
    ccbold=$(shell tput bold)
    ccgreen=$(shell tput setaf 2)
    ccred=$(shell tput setaf 1)
    ccso=$(shell tput smso)
