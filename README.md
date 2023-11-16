# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/coatl-dev/workflows/coatl.svg)](https://results.pre-commit.ci/latest/github/coatl-dev/workflows/coatl)

## Reusable workflows

Our main goal is to provide tools for maintainers working on Python 2 projects.

### Catalog

Workflows:

- [pre-commit](#githubworkflowspre-commityml)
- [pylint](#githubworkflowspylintyml)
- [pypi-upload](#githubworkflowspypi-uploadyml)
- [tox-docker](#githubworkflowstox-dockeryml)
- [tox-envs](#githubworkflowstox-envsyml)
- [tox-gh](#githubworkflowstox-ghyml)
- [tox](#githubworkflowstoxyml)

### .github/workflows/pre-commit.yml

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit`].

**Notes**:

- :warning: The latest `pre-commit` release requires `python>=3.8`.

**Inputs**:

- `cache` (`boolean`): If `true`, files and dependencies will be cached between
  workflow runs. Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/pre-commit.yml@v1.1.2
    with:
      skip-hooks: 'pylint'
```

### .github/workflows/pylint.yml

This workflow will install Python and invoke `pylint` to analyze your code.

**Notes**:

- :warning: The latest `pylint` release requires `python>=3.7.2`.

**Inputs**:

- `cache` (`boolean`): If `true`, files and dependencies will be cached between
  workflow runs. Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/pylint.yml@v1.1.2
```

### .github/workflows/pypi-upload.yml

This workflow allows you to build and upload your Python distribution packages
PyPI (or any other repository) using `build` and `twine`.

**Inputs**:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-latest`. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`, `ubuntu-latest`). Optional.
- `python-version` (`string`): The Python version to use for building and
  publishing the package. You may use MAJOR.MINOR or exact version. Defaults to
  `'3.11'`. Optional
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
    uses: coatl-dev/workflows/.github/workflows/pypi-upload.yml@v1.1.2
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

- `cache` (`boolean`): If `true`, files will be cached between workflow runs.
  Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/tox-docker.yml@v1.1.2
```

### .github/workflows/tox-envs.yml

This workflow will install Python and invoke tox envs based on the list of
Python versions.

**Notes**:

- :warning: The latest `tox` release requires `python>=3.7`.

**Inputs**:

- `cache` (`boolean`): If `true`, files and dependencies will be cached between
  workflow runs. Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/tox-envs.yml@v1.1.2
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox-gh.yml

This workflow will install Python and [`tox-gh`] and it will run the matching
`tox` environment based on the `gh` configuration section found in `tox.ini`.

**Notes**:

- :warning: The latest `tox-gh` release requires `python>=3.7`.

**Inputs**:

- `cache` (`boolean`): If `true`, files and dependencies will be cached between
  workflow runs. Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/tox-gh.yml@v1.1.2
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox.yml

This workflow will install Python and invoke `tox` to run all envs found in
[`env_list`].

**Notes**:

- :warning: The latest `tox` release requires `python>=3.7`.

**Inputs**:

- `cache` (`boolean`): If `true`, files and dependencies will be cached between
  workflow runs. Defaults to `true`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/tox.yml@v1.1.2
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
[testing end-of-life]: https://tox.wiki/en/latest/faq.html#testing-end-of-life-python-versions
