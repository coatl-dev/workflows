# workflows

## Reusable workflows

### .github/workflows/pypi-upload.yml

This allows you to upload your Python distribution packages in the `dist/`
directory to PyPI using `build` and `twine`.

#### Parameters

- `image`: Name of the VM Image to use for running the pipeline. Defaults to
  `ubuntu-20.04`. Options: (`ubuntu-20.04`, `ubuntu-22.04`).
- `python-version`: The Python version to use for building and publishing the
  package. Defaults to `"2.7"`. Options: (`"2.7"`, `"3.10"`).
- `pypi-token`: PyPI API token.

This action sets the proper `tox` env based on the Python version. For example:
`"3.10"` will run with `py310`, `"3.9"` will run `py39` and so forth.

#### Example

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/pypi-upload.yml@v0.0.1
    with:
      image: ubuntu-22.04
      python-version: "3.10"
      pypi-token: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-envs.yml

This job template will install Python and invoke tox envs based on the list of
Python versions.

Parameters:

- `python-versions`: (JSON list of strings) the list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`
- `image`: (default: `ubuntu-22.04`) passed through to [`runs-on`]

This action sets the proper `tox` env based on the Python version. For example:
`"3.10"` will run with `py310`, `"3.9"` will run `py39` and so forth.

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/tox-envs.yml@v0.0.1
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10"]'
```

### .github/workflows/tox-gh.yml

This job template will install Python and [`tox-dev/tox-gh`] and it will run
the matching env based on the `gh` configuration found in `tox.ini`.

Parameters:

- `python-versions`: (JSON list of strings) the list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`
- `image`: (default: `ubuntu-22.04`) passed through to [`runs-on`]

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
    uses: thecesrom/workflows/.github/workflows/tox-gh.yml@v0.0.1
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10"]'
```

### .github/workflows/tox.yml

This job template will install Python and invoke tox to run all envs found in
`env_list`.

Parameters:

- `image` (`string`): Passed through to [`runs-on`]. Defaults to
  `ubuntu-22.04`. Optional.
- `pre-commit` (`boolean`): If set to `true`, this means `pre-commit`
  will be ran inside a `tox` env. Defaults to `false`. Optional.
- `python-version` (`string`): Python version passed through to
  [`actions/setup-python`]. Defaults to `"3.10"`. Optional.

Example:

```yaml
jobs:
  main:
    uses: thecesrom/workflows/.github/workflows/tox.yml@v0.0.1
    with:
      image: ubuntu-20.04
      pre-commit: true
```
