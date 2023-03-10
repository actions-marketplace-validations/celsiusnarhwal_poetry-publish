# Publish With Poetry

This GitHub Action publishes a [Poetry](https://python-poetry.org/) project to any Python package repository.

## Features

- Built-in support for [PyPI](https://pypi.org) and [TestPyPI](https://test.pypi.org)
- Support for both username/passord and token authentication
- Customizable build process
- Makes no assumptions about whether Python or Poetry are already installed

## Usage

### Inputs

| **Name**         | **Required?** | **Description**                                                                                                                                                                      | **Default** |
|------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `python-version` | No[^1]        | The version of Python to use. If you don't provide this, the action will use whatever version is in $PATH. Poetry requires Python 3.7 or later.                                      | None        |
| `poetry-version` | No            | The version of Poetry to use. If you don't provide this, the action will use the version of Poetry in $PATH if one exists and install the latest stable version of Poetry otherwise. | None        |
| `repo`           | No            | The repository to publish to. Must be either "pypi", "testpypi", or an appropriate URL. Defaults to "pypi".                                                                          | pypi        |
| `username`       | No[^2]        | The username to use for the publication repository.                                                                                                                                  | ""          |
| `password`       | No[^2]        | The password to use for the publication repository.                                                                                                                                  | ""          |
| `token`          | No[^2]        | The token to use for the publication repository. If provided, this will be preferred over a username and password.                                                                   | ""          |
| `build`          | No            | Whether to build the package before publishing it. Defaults to "true".                                                                                                               | true        |
| `build-format`   | No            | The format to build the package in. Must be either "sdist" or "wheel". Both formats will be built if this isn't specified.                                                           | ""          |
| `dependencies`   | No            | A space-separated list of dependency groups to install at build time. Groups not listed will not be installed. If `build` is "false", this option has no effect. Defaults to "main". | main        |
| `extras`         | No            | A space-separated list of extras to install at build time. If `build` is "false", this option has no effect. Set this to "all" to install all extras.                                | ""          |

### Example

```yaml
steps:
  - name: Checkout Repository  # You must checkout your repository first.
    uses: actions/checkout@v3

  - name: Publish Package
    uses: celsiusnarhwal/poetry-publish@v1
    with:
      python-version: 3.11
      poetry-version: 1.3.1
      token: ${{ secrets.PYPI_TOKEN }}
      build: true
```

## License

Publish With Poetry is licensed under
the [MIT License](https://github.com/celsiusnarhwal/poetry-publish/blob/main/LICENSE.md).

[^1]: *Some* version of Python comes preinstalled on all GitHub-hosted runners, but it's best practice to explicitly
specify one that you know is compatible with both Poetry and your project. Behind the scenes, this action uses
[actions/setup-python](https://github.com/actions/setup-python).

[^2]: `username`, `password`, and `token` are defined as optional so you can choose how to authenticate to the
publication repository. You must provide either a username and password or a token or the action will fail.