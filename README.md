# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/coatl-dev/workflows/coatl.svg)](https://results.pre-commit.ci/latest/github/coatl-dev/workflows/coatl)

## Reusable workflows

Our main goal is to provide tools for maintainers working on Python 2 projects.

### Catalog

Workflows:

- [black](#githubworkflowsblackyml)
- [flake8](#githubworkflowsflake8yml)
- [mypy](#githubworkflowsmypyyml)
- [pre-commit](#githubworkflowspre-commityml)
- [pylint](#githubworkflowspylintyml)
- [pypi-upload](#githubworkflowspypi-uploadyml)
- [tox-docker](#githubworkflowstox-dockeryml)
- [tox-envs](#githubworkflowstox-envsyml)
- [tox-gh](#githubworkflowstox-ghyml)
- [tox](#githubworkflowstoxyml)

### .github/workflows/black.yml

This workflow will install Python 3.11 and invoke `black` to run against your
Python 2.7 code.

**Notes**:

- :information_source: This essentially installs `black[python2]==21.9b0`, which
  is the last version of black that did not warn about Python 2 deprecation.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Defaults to `'.'`. Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

`pyproject.toml`:

```toml
[tool.black]
line-length = 88
target-version = ["py27"]
```

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/black.yml@v1.0.0
    with:
      sources-root: 'src'
```

### .github/workflows/flake8.yml

This workflow will install Python and invoke `flake8` to run against your
Python 2.7 code.

**Notes**:

- :information_source: This essentially installs `flake8==5.0.4`, which includes
  the last version of `pyflakes` to support Python 2 [type comments].

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `'3.11'`. Options: (`'3.7'` through
  `'3.12'`). Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Defaults to `'.'`. Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/flake8.yml@v1.0.0
    with:
      sources-root: 'src'
```

### .github/workflows/mypy.yml

This workflow will install Python and invoke `mypy` to run against your
Python 2.7 code.

**Notes**:

- :information_source: This essentially installs `mypy[python2]==0.971`,
  which was the last release officially supporting Python 2.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `mypy-requirements` (`string`): Path to additional requirements file
  necessary for running mypy. Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `'3.11'`. Options: (`'3.7'` through
  `'3.12'`). Optional.
- `sources-root` (`string`): The root directory for your source code. E.g.
  `src`. Required.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

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
    uses: coatl-dev/workflows/.github/workflows/mypy.yml@v1.0.0
    with:
      mypy-requirements: 'requirements/typecheck.txt'
      sources-root: 'src'
```

### .github/workflows/pre-commit.yml

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit`].

**Notes**:

- :warning: The latest `pre-commit` release requires `python>=3.8`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `'3.11'`. Options: (`'3.8'` through
  `'3.12'`). Optional.
- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pre-commit.yml@v1.0.0
    with:
      skip-hooks: 'pylint'
```

### .github/workflows/pylint.yml

This workflow will install Python and invoke `pylint` to analyze your code.

**Notes**:

- :warning: The latest `pylint` release requires `python>=3.7.2`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `'3.11'`. Options: (`'3.7'` through
  `'3.12'`). Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pylint.yml@v1.0.0
```

### .github/workflows/pypi-upload.yml

This workflow allows you to build and upload your Python distribution packages
PyPI (or any other repository) using `build` and `twine`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`). Optional.
- `python-version` (`string`): The Python version to use for building and
  publishing the package. Defaults to `'3.11'`. Options: (`'3.7'` through
  `'3.12'`). Optional.
- `check` (`boolean`): Check metadata with twine before uploading. Defaults to
  `true`. Optional.
- `url` (`string`): The repository (package index) URL to upload the package to.
  Defaults to `'https://upload.pypi.org/legacy/'`. Optional.
- `username` (`string`): The username to authenticate to the repository (package
  index) as. Defaults to `'__token__'`. Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

Secrets:

- `password` (`secret`): The password to authenticate to the repository (package
  index) with. This can also be a token. Required.

**Recommendations**:

If you would like to build and upload a Python 2 project, you may use our
action: [action-pypi-upload](https://github.com/coatl-dev/action-pypi-upload).

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pypi-upload.yml@v1.0.0
    with:
      python-version: '3.11'
    secrets:
      password: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-docker.yml

This workflow will install the latest version of `tox` to run all envs found in
[`env_list`].

**Notes**:

- This workflow uses the [`coatldev/six:3.11`] Docker image, which comes with
  Python 2.7 and 3.11.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `pre-commit` (`boolean`): If set to `true`, this means `pre-commit` will be
  invoked inside a `tox` environment. Defaults to `false`. Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

```ini
[tox]
requires =
    virtualenv<20.22.0
```

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-docker.yml@v1.0.0
```

### .github/workflows/tox-envs.yml

This workflow will install Python and invoke tox envs based on the list of
Python versions.

**Notes**:

- :warning: The latest `tox` release requires `python>=3.7`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

This action sets the proper `tox` env based on the Python version. For example:
`'3.10'` will run `py310`, `'3.9'` will run `py39` and so forth.

**Recommendations**:

When [testing end-of-life] Python, e.g. 2.7, you need to add the following
`requires` statement to your `tox.ini` configuration file:

```ini
[tox]
requires =
    virtualenv<20.22.0
```

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-envs.yml@v1.0.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox-gh.yml

This workflow will install Python and [`tox-gh`] and it will run the matching
`tox` environment based on the `gh` configuration section found in `tox.ini`.

**Notes**:

- :warning: The latest `tox-gh` release requires `python>=3.7`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

tox.ini:

```ini
[gh]
python =
    3.7 = py37
    3.8 = py38
    3.9 = py39
    3.10 = py310, install, typecheck
    3.11 = py311
    3.12 = py312
```

and on your workflow:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-gh.yml@v1.0.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox.yml

This workflow will install Python and invoke `tox` to run all envs found in
[`env_list`].

**Notes**:

- :warning: The latest `tox` release requires `python>=3.7`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`, `ubuntu-22.04`,
  `ubuntu-latest`). Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `'3.11'`. Options: (`'3.7'` through
  `'3.12'`). Optional.
- `timeout` (`number`): The maximum number of minutes to let a job run before
  GitHub automatically cancels it. Defaults to `15`. Optional.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox.yml@v1.0.0
```

[`actions/setup-python`]: https://github.com/actions/setup-python
[`coatldev/six:3.11`]: https://hub.docker.com/r/coatldev/six
[`env_list`]: https://tox.wiki/en/latest/config.html#env_list
[`local hooks`]: https://pre-commit.com/#repository-local-hooks
[`pre-commit`]: https://pre-commit.com/
[`pre-commit.ci`]: https://pre-commit.ci/
[`runs-on`]: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idruns-on
[Temporarily disabling hooks]: https://pre-commit.com/#temporarily-disabling-hooks
[`tox-gh`]: https://github.com/tox-dev/tox-gh
[type comments]: https://peps.python.org/pep-0484/#type-comments
[testing end-of-life]: https://tox.wiki/en/latest/faq.html#testing-end-of-life-python-versions
