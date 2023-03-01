# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/coatl-dev/workflows/main.svg)](https://results.pre-commit.ci/latest/github/coatl-dev/workflows/main)

## Reusable workflows

### .github/workflows/black.yml

This workflow will install Python and invoke `black` to run against your
Python 2.7 code.

> Note: This essentially installs `black[python2]==21.9b0`, which is the last
version of black that did not warn about Python 2 deprecation. This release
requires `python>=3.6.2`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Optional. Defaults to `"."`.

Example:

`pyproject.toml`:

```toml
[tool.black]
line-length = 88
target-version = ["py27"]
```

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/black.yml@v2.2.0
    with:
      sources-root: "src"
```

### .github/workflows/flake8.yml

This workflow will install Python and invoke `flake8` to run against your
Python 2.7 code.

> Note: This essentially installs `flake8==5.0.4`, which includes the last
version of `pyflakes` to support Python 2 [type comments]. This requires
`python>=3.6.1`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Optional. Defaults to `"."`.

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/flake8.yml@v2.2.0
    with:
      sources-root: "src"
```

### .github/workflows/mypy.yml

This workflow will install Python and invoke `mypy` to run against your
Python 2.7 code.

> Note: This essentially installs `mypy[python2]==0.971`, which was the last
release officially supporting Python 2. This requires `python>=3.6`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `mypy-requirements` (`string`): Path to additional requirements file
  necessary for running mypy. Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Required.

Example:

`mypy.ini`:

```ini
[mypy]
python_version = 2.7
mypy_path = src
enable_error_code = ignore-without-code
```

`requirements/typecheck.txt`:

```text
types-enum34
```

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/mypy.yml@v2.2.0
    with:
      mypy-requirements: "requirements/typecheck.txt"
      sources-root: "src"
```

### .github/workflows/pre-commit.yml

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit`].

> Note: The latest `pre-commit` release requires `python>=3.8`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.
- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pre-commit.yml@v2.2.0
    with:
      python-version: "3.11"
      skip-hooks: "mypy,pylint"
```

### .github/workflows/pylint.yml

This workflow will install Python and invoke `pylint` to analyze your code.

> Note: The latest `pylint` release requires `python>=3.7.2`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pylint.yml@v2.2.0
    with:
      image: ubuntu-latest
      python-version: "3.11"
```

### .github/workflows/pypi-upload.yml

This workflow allows you to upload your Python distribution packages in the
`dist/` directory to PyPI using `build` and `twine`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-version` (`string`): The Python version to use for building and
  publishing the package. Defaults to `"2.7"`. Optional.

Secrets:

- `pypi-token` (`secret`): PyPI API token. Required.

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pypi-upload.yml@v2.2.0
    with:
      image: ubuntu-22.04
      python-version: "3.11"
    secrets:
      pypi-token: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-envs.yml

This workflow will install Python and invoke tox envs based on the list of
Python versions.

Note: The latest `tox` release requires `python>=3.7`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.

This action sets the proper `tox` env based on the Python version. For example:
`"3.10"` will run `py310`, `"3.9"` will run `py39` and so forth.

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-envs.yml@v2.2.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11"]'
```

### .github/workflows/tox-gh.yml

This workflow will install Python and [`tox-gh`] and it will run the matching
`tox` environment based on the `gh` configuration section found in `tox.ini`.

Note: The latest `tox-gh` release requires `python>=3.7`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.

Example:

tox.ini:

```ini
[gh]
python =
    3.7 = py37
    3.8 = py38
    3.9 = py39
    3.10 = py310, install, typecheck
    3.11 = py311
```

and on your workflow:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-gh.yml@v2.2.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11"]'
```

### .github/workflows/tox.yml

This workflow will install Python and invoke `tox` to run all envs found in
[`env_list`].

Note: The latest `tox` release requires `python>=3.7`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-20.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`).
- `pre-commit` (`boolean`): If set to `true`, this means `pre-commit` will be
  invoked inside a `tox` environment. Defaults to `false`. Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.

Example:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox.yml@v2.2.0
    with:
      pre-commit: true
```

[`actions/setup-python`]: https://github.com/actions/setup-python
[`env_list`]: https://tox.wiki/en/latest/config.html#env_list
[`local hooks`]: https://pre-commit.com/#repository-local-hooks
[`pre-commit`]: https://pre-commit.com/
[`pre-commit.ci`]: https://pre-commit.ci/
[`runs-on`]: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idruns-on
[Temporarily disabling hooks]: https://pre-commit.com/#temporarily-disabling-hooks
[`tox-gh`]: https://github.com/tox-dev/tox-gh
[type comments]: https://peps.python.org/pep-0484/#type-comments
