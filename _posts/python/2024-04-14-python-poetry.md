---
layout: post
title: Python Package Management - Poetry
category: Python
tag: [python package management, poetry]
---

## Install Poetry
```bash
curl -sSL https://install.python-poetry.org | python3
```

* check the version
    ```bash
    $ poetry --version
    ```

## Start project with peotry 

#### Create a project with poetry
```bash
poetry new [PROJECT_NAME]
```

#### Init Poetry with the existed project
```bash
poetry init
```

You can see `pyproject.toml` file.

> Yet, `poetry.lock` does not exist. This will be created after installing a package.

## Manage packages

* Add 
    ```bash
    poetry add [PACKAGE_NAME]
    ```

    * Specify the version
        ```bash
        poetry add [PACKAGE_NAME]@[VERSION]
        ```

        ```bash
        poetry add "[PACKAGE_NAME]=[VERSION]"
        ```

        * For the latest version,
            ```bash
            poetry add [PACKAGE_NAME]@latest
            ```

    * Add the pacakges in git
        ```bash
        poetry add [GIT_CLONE_URL]
        ```

    * Add the pacakges in directory
        ```bash
        poetry add ./my-package/
        poetry add ./my-package/dist/my-package-0.1.0.tar.gz
        poetry add ./my-package/dist/my-package-0.1.0.whl     
        ```

    * Add for `dev`
        ```bash
        poetry add [PACKAGE_NAME] --dev
        ```

The added package information is added at `[tool.poetry.dependencies]` in `pyproject.toml` and, `poetry.lock` is also created.

* Remove
    ```bash
    poetry remove [PACKAGE_NAME]
    ```

* Export 
    ```bash 
    poetry export -f requirements.txt > requirements.txt
    ```

* Show
    ```bash
    poetry show
    ```

    * Without dev,
        ```bash
        poetry show --no-dev
        ```

    * A pacakge
        ```bash
        poetry show django
        ```
    * The latest
        ```bash
        poetry show --latest (-l)
        ```
    * Outdate packages
        ```bash
        poetry show --outdate (-o)
        ```
    * Dependency tree
        ```bash
        poetry show --tree
        ```        

## Build the package
```
poetry build
```

It will create `tarball` and `wheel` packages.

## Configurations

* To see the configurations
    ```
    poetry config --list
    ```

## Publish the package 
```
poetry publish
```

It will publish the package with the name that you typed into the `PyPI` server and this requires the account at `PyPI`.

* To publish into a private `PyPI` server

    

## Install packages by poetry
```bash 
poetry install
```

Based on the dependencies in `pyproject.toml`, it will install all packages with specified version. When the `poetry.lock` does not exist, it will create `poetry.lock` and then, install the packages.

* Install w/o dev dependencies 
    ```bash
    poetry install --no-dev
    ```

* Install pacakage additionally by `--extra` or `-E`
    ```bash
    poetry install --extras "mysql redis"
    ```

    ```bash
    poerty install -E mysql -E redis
    ```

## Update dependencies
```bash
poetry update
```

* Update a pacakge 
    ```bash
    poetry update [PACKAGE_NAME_1] [PACKAGE_NAME_1]
    ```

* Only update `poetry.lock`
    ```bash
    poetry update --lock
    ```

