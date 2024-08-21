# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/coatl-dev/workflows/coatl.svg)](https://results.pre-commit.ci/latest/github/coatl-dev/workflows/coatl)

## Reusable workflows

Our main goal is to provide tools for maintainers working on Python 2 projects.

### Catalog

Workflows:

- [docker-publish-multi-platform](#githubworkflowsdocker-publish-multi-platform)
- [pip-compile-upgrade](#githubworkflowspip-compile-upgrade)
- [pre-commit-autoupdate](#githubworkflowspre-commit-autoupdate)
- [pre-commit](#githubworkflowspre-commityml)
- [pylint](#githubworkflowspylintyml)
- [pypi-upload](#githubworkflowspypi-uploadyml)
- [tox-docker](#githubworkflowstox-dockeryml)
- [tox-envs](#githubworkflowstox-envsyml)
- [tox-gh](#githubworkflowstox-ghyml)
- [tox](#githubworkflowstoxyml)

### .github/workflows/docker-publish-multi-platform

GitHub action for using a matrix strategy to distribute the build for multiple
platforms and publish to a Docker registry of your choice (Docker Hub, ghcr.io
or quay.io).

**Inputs**:

- `registry-image` (`string`): Docker image to use as base name for tags.
- `metadata-tags` (`string`): List of tags as key-value pair attributes.
  Optional.
- `registry-address` (`string`): Server address of Docker registry. If not set
  then will default to Docker registry. Optional.
- `registry-username` (`string`): Username for authenticating to the Docker
  registry.
- `build-platforms` (`string`): List of target platforms for build. Defaults to
  `'["linux/amd64", "linux/arm64"]'`.Optional.
- `build-context` (`string`): Build's context is the set of files located in the
  specified PATH or URL. Optional.
- `build-file` (`string`): Path to the Dockerfile. Optional.
- `build-cache-key` (`string`): An explicit key for a cache entry. This will be
  used in conjunction with the platform set in `build-platforms`, e.g.
  `coatl-linux-amd64`. Defaults to `coatl`. Optional.

**Secrets**:

- `registry-password` (`secret`): Password or personal access token for
  authenticating the Docker registry.

**Example**:

```yml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/docker-publish-multi-platform.yml@v5.0.0
    with:
      registry-image: user/app
      metadata-tags: |
        type=semver,pattern={{version}}
        type=semver,pattern={{major}}.{{minor}}
        type=semver,pattern={{major}}
      registry-username: ${{ vars.DOCKERHUB_USERNAME }}
      build-platforms: '["linux/amd64", "linux/arm64", "linux/arm/v7"]'
      build-cache-key: mykey
    secrets:
      registry-password: ${{ secrets.DOCKERHUB_TOKEN }}
```

### .github/workflows/pip-compile-upgrade

GitHub action for running `pip-compile upgrade` on your Python 2 and 3
requirements.

**Inputs**:

- `path` (`string`): A file or location of the requirement file(s).
- `python-version` (`string`): Python version to use for installing `pip-tools`.
  You may use MAJOR.MINOR or exact version. Defaults to `'3.12'`. Optional.
- `pr-create` (`string`): Whether to create a Pull Request. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.
- `pr-commit-message` (`string`): Use the given message as the commit message.
  Defaults to `'chore(requirements): pip-compile upgrade'`. Optional.
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `sign-commits` (`string`): Whether to sign Git commits. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.

**Secrets**:

- `gh-token` (`secret`): GitHub token. Required when creating PRs, otherwise is
  optional.
- `gpg-sign-passphrase` (`secret`): GPG private key passphrase. Required when
  signing commits, otherwise is optional.
- `gpg-sign-private-key` (`secret`): GPG private key exported as an ASCII
  armored version. Required when signing commits, otherwise is optional.

**Example**:

```yml
name: pip-compile-upgrade

on:
  schedule:
    - cron: '0 20  * * 1'
  workflow_dispatch:

jobs:
  pip-compile-upgrade:
    uses: coatl-dev/workflows/.github/workflows/pip-compile-upgrade.yml@v5.0.0
    with:
      path: requirements.txt
    secrets:
      gh-token: ${{ secrets.GH_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

### .github/workflows/pre-commit-autoupdate

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit autoupdate`].

**Inputs**:

- `pr-base-branch` (`string`): The branch into which you want your code merged.
  Defaults to `'main'`. Required when `pr-create` is set to `'yes'`, otherwise
  is optional.
- `pr-create` (`string`): Whether to create a Pull Request. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `sign-commits` (`string`): Whether to sign Git commits. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.
- `skip-repos` (`string`): A list of repos to exclude from autoupdate. The repos
  must be separated by a "pipe" character `'|'`. Defaults to `''`. Optional.

**Secrets**:

- `gh-token` (`secret`): GitHub token. Required when creating PRs, otherwise is
  optional.
- `gpg-sign-passphrase` (`secret`): GPG private key passphrase. Required when
  signing commits, otherwise is optional.
- `gpg-sign-private-key` (`secret`): GPG private key exported as an ASCII
  armored version. Required when signing commits, otherwise is optional.

**Example**:

```yml
name: pre-commit-autoupdate

on:
  schedule:
    - cron: '0 20 * * 1'
  workflow_dispatch:

jobs:
  pre-commit-autoupdate:
    uses: coatl-dev/workflows/.github/workflows/pre-commit-autoupdate.yml@v5.0.0
    with:
      skip-repos: 'flake8'
    secrets:
      gh-token: ${{ secrets.GH_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

### .github/workflows/pre-commit.yml

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit`].

**Inputs**:

- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pre-commit.yml@v5.0.0
    with:
      skip-hooks: 'pylint'
```

### .github/workflows/pylint.yml

This workflow will install Python and invoke `pylint` to analyze your code.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pylint.yml@v5.0.0
```

### .github/workflows/pypi-upload.yml

This workflow allows you to build and upload your Python distribution packages
PyPI (or any other repository) using `build` and `twine`.

**Inputs**:

- `python-version` (`string`): The Python version to use for building and
  publishing the package. You may use MAJOR.MINOR or exact version. Defaults to
  `'3.12'`. Optional
- `check` (`boolean`): Check metadata with twine before uploading. Defaults to
  `true`. Optional.
- `url` (`string`): The repository (package index) URL to upload the package to.
  Defaults to `'https://upload.pypi.org/legacy/'`. Optional.
- `username` (`string`): The username to authenticate to the repository (package
  index) as. Defaults to `'__token__'`. Optional.

Secrets:

- `password` (`secret`): The password to authenticate to the repository (package
  index) with. This can also be a token. Required.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pypi-upload.yml@v5.0.0
    with:
      python-version: '3.11'
    secrets:
      password: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-docker.yml

This workflow will install the latest version of `tox` to run all envs found in
[`env_list`].

> [!NOTE]
> This workflow uses the [`coatldev/six`] Docker image, which comes with
> Python 3.12 and 2.7.18.

**Example**:

```ini
[tox]
requires =
    tox>=4.2
    virtualenv<20.22.0
```

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-docker.yml@v5.0.0
```

### .github/workflows/tox-envs.yml

This workflow will install Python and invoke tox envs based on the list of
Python versions.

**Inputs**:

- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.

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
    uses: coatl-dev/workflows/.github/workflows/tox-envs.yml@v5.0.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox-gh.yml

This workflow will install Python and [`tox-gh`] and it will run the matching
`tox` environment based on the `gh` configuration section found in `tox.ini`.

**Inputs**:

- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.

> [!IMPORTANT]
> The latest `tox-gh` release requires `python>=3.7`.

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
    uses: coatl-dev/workflows/.github/workflows/tox-gh.yml@v5.0.0
    with:
      python-versions: '["3.7", "3.8", "3.9", "3.10", "3.11", "3.12"]'
```

### .github/workflows/tox.yml

This workflow will install Python and invoke `tox` to run all envs found in
[`env_list`].

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox.yml@v5.0.0
```

[`actions/setup-python`]: https://github.com/actions/setup-python
[`coatldev/six`]: https://hub.docker.com/r/coatldev/six
[`env_list`]: https://tox.wiki/en/latest/config.html#env_list
[`local hooks`]: https://pre-commit.com/#repository-local-hooks
[`pre-commit`]: https://pre-commit.com/
[`pre-commit autoupdate`]: https://pre-commit.com/#pre-commit-autoupdate
[`pre-commit.ci`]: https://pre-commit.ci/
[Temporarily disabling hooks]: https://pre-commit.com/#temporarily-disabling-hooks
[`tox-gh`]: https://github.com/tox-dev/tox-gh
[testing end-of-life]: https://tox.wiki/en/latest/faq.html#testing-end-of-life-python-versions
