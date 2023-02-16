# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/thecesrom/workflows/main.svg)](https://results.pre-commit.ci/latest/github/thecesrom/workflows/main)

## Reusable workflows

### .github/workflows/pre-commit.yml

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this job
template to install Python and invoke [`pre-commit`].

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-22.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`).
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.
- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/pre-commit.yml@v1.2.0
    with:
      python-version: "3.11"
      skip-hooks: "mypy,pylint"
```

### .github/workflows/pypi-upload.yml

This allows you to upload your Python distribution packages in the `dist/`
directory to PyPI using `build` and `twine`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-22.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`).
- `python-version` (`string`): The Python version to use for building and
  publishing the package. Defaults to `"2.7"`. Optional.

Secrets:

- `pypi-token` (`secret`): PyPI API token. Required.

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/pypi-upload.yml@v1.2.0
    with:
      image: ubuntu-22.04
      python-version: "3.10"
    secrets:
      pypi-token: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-envs.yml

This job template will install Python and invoke tox envs based on the list of
Python versions.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-22.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`).
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.

This action sets the proper `tox` env based on the Python version. For example:
`"3.10"` will run `py310`, `"3.9"` will run `py39` and so forth.

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/tox-envs.yml@v1.2.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10"]'
```

### .github/workflows/tox-gh.yml

This job template will install Python and [`tox-gh`] and it will run the
matching `tox` environment based on the `gh` configuration section found in
`tox.ini`.

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-22.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`).
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
```

and on your workflow:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/tox-gh.yml@v1.2.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10"]'
```

### .github/workflows/tox.yml

This job template will install Python and invoke `tox` to run all envs found in
[`env_list`].

Inputs:

- `image` (`string`): Name of the VM Image passed through to [`runs-on`].
  Defaults to `ubuntu-22.04`. Optional. Options: (`ubuntu-20.04`,
  `ubuntu-22.04`).
- `pre-commit` (`boolean`): If set to `true`, this means `pre-commit` will be
  invoked inside a `tox` environment. Defaults to `false`. Optional.
- `python-version` (`string`): Python version input passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/tox.yml@v1.2.0
    with:
      image: ubuntu-20.04
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
