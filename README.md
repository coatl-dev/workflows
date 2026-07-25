# workflows

[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/coatl-dev/workflows/coatl.svg)](https://results.pre-commit.ci/latest/github/coatl-dev/workflows/coatl)

## Reusable workflows

Our main goal is to provide tools for maintainers working on Python 2 projects.

### Catalog

Workflows:

- [docker-build-push-multi-platform](#githubworkflowsdocker-build-push-multi-platform)
- [docker-build-push-multi-registry](#githubworkflowsdocker-build-push-multi-registry)
- [pip-compile-upgrade](#githubworkflowspip-compile-upgrade)
- [pre-commit-autoupdate](#githubworkflowspre-commit-autoupdate)
- [pre-commit](#githubworkflowspre-commityml)
- [prek-autoupdate](#githubworkflowsprek-autoupdate)
- [prek](#githubworkflowsprekyml)
- [pylint](#githubworkflowspylintyml)
- [pypi-upload](#githubworkflowspypi-uploadyml)
- [tox-docker](#githubworkflowstox-dockeryml)
- [tox-gh](#githubworkflowstox-ghyml)
- [tox](#githubworkflowstoxyml)
- [uv-pip-compile-upgrade](#githubworkflowsuv-pip-compile-upgrade)

### .github/workflows/docker-build-push-multi-platform

GitHub action for using a matrix strategy to distribute the build for
`linux/amd64` and `linux/arm64`, and publish to a Docker registry of your choice
(Docker Hub, ghcr.io or quay.io).

**Inputs**:

- `registry-image` (`string`): Docker image to use as base name for tags.
- `metadata-tags` (`string`): List of tags as key-value pair attributes.
  Optional.
- `registry-address` (`string`): Server address of Docker registry. If not set
  then will default to Docker registry. Optional.
- `registry-username` (`string`): Username for authenticating to the Docker
  registry.
- `build-context` (`string`): Build's context is the set of files located in the
  specified PATH or URL. Optional.
- `build-file` (`string`): Path to the Dockerfile. Optional.
- `build-provenance` (`boolean`): Generate provenance attestation for the build.
  Defaults to `false`. Optional.
- `build-cache-key` (`string`): An explicit key for a cache entry. This will be
  used in conjunction with the platform set in `build-platforms`, e.g.
  `coatl-linux-amd64`. Defaults to `coatl`. Optional.
- `build-digest-key` (`string`): Name of the build digest. This will be used in
  conjunction with the platform set in `build-platforms`, e.g.
  `coatl-linux-amd64`. Defaults to `coatl`. Optional.

**Secrets**:

- `registry-password` (`secret`): Password or personal access token for
  authenticating the Docker registry.

**Example**:

```yml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/docker-build-push-multi-platform.yml@v7.0.22
    with:
      registry-image: user/app
      metadata-tags: |
        type=semver,pattern={{version}}
        type=semver,pattern={{major}}.{{minor}}
        type=semver,pattern={{major}}
      registry-username: ${{ vars.DOCKERHUB_USERNAME }}
      build-context: "{{defaultContext}}:mysubdir"
      build-provenance: true
      build-cache-key: mykey
      build-digest-key: mydigest
    secrets:
      registry-password: ${{ secrets.DOCKERHUB_TOKEN }}
```

### .github/workflows/docker-build-push-multi-registry

GitHub action for using a matrix strategy to distribute the build for
`linux/amd64` and `linux/arm64`, and publish to Docker Hub and quay.io.

**Inputs**:

- `dockerhub-repo` (`string`): Docker Hub repository to push the image to.
- `dockerhub-username` (`string`): Username for authenticating to Docker Hub.
- `quay-repo` (`string`): Quay repository to push the image to.
- `quay-username` (`string`): Username for authenticating to Quay.
- `build-context` (`string`): Build's context is the set of files located in the
  specified PATH or URL. Optional.
- `build-file` (`string`): Path to the Dockerfile. Optional.
- `build-cache-key` (`string`): An explicit key for a cache entry. This will be
  used in conjunction with the platform set in `build-platforms`, e.g.
  `coatl-linux-amd64`. Defaults to `coatl`. Optional.
- `build-digest-key` (`string`): Name of the build digest. This will be used in
  conjunction with the platform set in `build-platforms`, e.g.
  `coatl-linux-amd64`. Defaults to `coatl`. Optional.
- `metadata-tags` (`string`): List of tags as key-value pair attributes.
  Optional.

**Secrets**:

- `dockerhub-password` (`secret`): Password or personal access token for
  authenticating against Docker Hub.
- `quay-password` (`secret`): Password or personal access token for
  authenticating against Quay.

**Example**:

```yml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/docker-build-push-multi-registry.yml@v7.0.22
    with:
      dockerhub-repo: user/app
      dockerhub-username: ${{ vars.DOCKERHUB_USERNAME }}
      quay-repo: quay.io/user/app
      quay-username: ${{ vars.QUAY_USERNAME }}
      build-context: "{{defaultContext}}:mysubdir"
      build-cache-key: mykey
      build-digest-key: mydigest
      metadata-tags: |
        type=semver,pattern={{version}}
        type=semver,pattern={{major}}.{{minor}}
        type=semver,pattern={{major}}
    secrets:
      dockerhub-password: ${{ secrets.DOCKERHUB_TOKEN }}
      quay-password: ${{ secrets.QUAY_ROBOT_TOKEN }}
```

### .github/workflows/pip-compile-upgrade

GitHub action for running `pip-compile upgrade` on your Python 2.7 requirements.

**Inputs**:

- `base-branch` (`string`): The branch to use as the base for the PR.
- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `extra-args` (`string`): Extra arguments to pass to `pip-compile`. Optional.
  Defaults to `''`.
- `path` (`string`): The location of the requirement file(s).
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `pr-branch` (`string`): The branch to use for the submitting the PR. Defaults
  to `'coatl-dev-pip-compile-upgrade'`. Optional.
- `pr-commit-message` (`string`): Use the given message as the commit message.
  Defaults to `'chore(requirements): pip-compile upgrade'`. Optional.
- `pr-delete-branch` (`string`): Delete the local and remote branch after merge.
  Options: `'yes'`, `'no'`. Defaults to `'no'`. Optional.
- `sign-commits` (`string`): Whether to sign Git commits. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

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
    uses: coatl-dev/workflows/.github/workflows/pip-compile-upgrade.yml@v7.0.22
    with:
      path: requirements.txt
      pr-create-additional-args: develop
    secrets:
      gh-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

### .github/workflows/pre-commit-autoupdate

If you [cannot/do not want to] benefit from [`pre-commit.ci`], use this workflow
to install Python and invoke [`pre-commit autoupdate`].

**Inputs**:

- `autoupdate-branch` (`string`): branch to send autoupdate PRs to. By default,
  this will update the default branch of the repository.
- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `pr-branch` (`string`): The branch to use for autoupdate. Defaults to
  `'coatl-dev-pre-commit-autoupdate'`. Optional.
- `pr-delete-branch` (`string`): Delete the local and remote branch after merge.
  Options: `'yes'`, `'no'`. Defaults to `'no'`. Optional.
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
    uses: coatl-dev/workflows/.github/workflows/pre-commit-autoupdate.yml@v7.0.22
    with:
      skip-repos: 'flake8'
      autoupdate-branch: 'develop'
    secrets:
      gh-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

### .github/workflows/pre-commit.yml

This workflow will install Python and invoke `pre-commit` to run hooks on all
the files in the repo.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pre-commit.yml@v7.0.22
    with:
      skip-hooks: 'pylint'
```

### .github/workflows/prek-autoupdate

This workflow will install Python and invoke `prek` to auto-update the `rev`
field of repositories in the config file to the latest.

**Inputs**:

- `autoupdate-branch` (`string`): branch to send autoupdate PRs to. By default,
  this will update the default branch of the repository.
- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `pr-delete-branch` (`string`): Delete the local and remote branch after merge.
  Options: `'yes'`, `'no'`. Defaults to `'no'`. Optional.
- `pr-branch` (`string`): The branch to use for autoupdate. Defaults to
  `'coatl-dev-prek-autoupdate'`. Optional.
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
name: prek-autoupdate

on:
  schedule:
    - cron: '0 20 * * 1'
  workflow_dispatch:

jobs:
  prek-autoupdate:
    uses: coatl-dev/workflows/.github/workflows/prek-autoupdate.yml@v7.0.22
    with:
      autoupdate-branch: 'develop'
    secrets:
      gh-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

### .github/workflows/prek.yml

This workflow will install Python and invoke `prek` to run hooks on all the
files in the repo.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `skip-hooks` (list[`string`]): A comma separated list of hook ids which will
  be disabled. Useful when your `pre-commit-config.yaml` file contains
  [`local hooks`]. Optional. See: [Temporarily disabling hooks].

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/prek.yml@v7.0.22
```

### .github/workflows/pylint.yml

This workflow will install Python and invoke `pylint` to analyze your code.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `path` (`string`): This can be a module, package, directory or a file.
  Optional.
- `extra-args` (`string`): Extra arguments to pass to `pylint`. Optional.
  Defaults to `''`.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pylint.yml@v7.0.22
    with:
      path: src
```

### .github/workflows/pypi-upload.yml

This workflow allows you to build and upload your Python distribution packages
PyPI (or any other repository) using `build` and `twine`.

> [!NOTE]
> This workflow uses the [`ghcr.io/coatl-dev/python-tools`] Docker image, which
> has tags for Python 2.7 and 3.12. E.g.,
> `ghcr.io/coatl-dev/python-tools:2.7-build`.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `python-version` (`string`): The Python version to use for building and
  publishing the package. Options: `'2.7'` or `'3.12'`. Defaults to `'2.7'`.
  Optional.
- `check` (`boolean`): Check metadata with twine before uploading. Defaults to
  `true`. Optional.
- `url` (`string`): The repository (package index) URL to upload the package to.
  Defaults to `'https://upload.pypi.org/legacy/'`. Optional.
- `username` (`string`): The username to authenticate to the repository (package
  index) as. Defaults to `'__token__'`. Optional.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

Secrets:

- `password` (`secret`): The password to authenticate to the repository (package
  index) with. This can also be a token. Required.

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/pypi-upload.yml@v7.0.22
    with:
      python-version: '3.12'
    secrets:
      password: ${{ secrets.PYPI_API_TOKEN }}
```

### .github/workflows/tox-docker.yml

This workflow will install the latest version of `tox` to run all envs found in
[`env_list`].

> [!NOTE]
> This workflow uses the [`coatldev/six`] Docker image, which comes with
> Python 3.12, 2.7.18 and `uv`.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `extra-args` (`string`): Extra arguments to pass to `tox`. Optional. Defaults
  to `''`.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.
- `uv-python` (`string`): The Python version to use with `uv`. Optional.
  Defaults to `'3.14'`.

**Recommendations**:

When [testing end-of-life] Python, e.g. 2.7, you need to add the following
`requires` statement to your `tox.ini` configuration file:

```ini
[tox]
requires =
    tox>=4.2
    virtualenv<20.22.0
```

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
    uses: coatl-dev/workflows/.github/workflows/tox-docker.yml@v7.0.22
```

### .github/workflows/tox-gh.yml

This workflow will install Python and [`tox-gh`] and it will run the matching
`tox` environment based on the `gh` configuration section found in `tox.ini`.

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `python-versions` (list[`string`]): A list of Python versions passed
  through to [`actions/setup-python`]'s `python-version`. Required.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

> [!IMPORTANT]
> The latest `tox-gh` release requires `python>=3.9`.

**Example**:

tox.ini:

```ini
[gh]
python =
    3.9 = py39
    3.10 = py310
    3.11 = py311
    3.12 = py312, typecheck
    3.13 = py313
    3.14 = py314, install
```

and on your workflow:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox-gh.yml@v7.0.22
    with:
      python-versions: '["3.9", "3.10", "3.11", "3.12", "3.13", "3.14"]'
```

### .github/workflows/tox.yml

This workflow will install Python and invoke `tox` to run all envs found in
[`env_list`].

**Inputs**:

- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `python-versions` (list[`string`]): Version range or exact version of Python
  to use, using SemVer's version range syntax. Required.
- `extra-args` (`string`): Extra arguments to pass to `tox`. Optional. Defaults
  to `''`.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

> [!NOTE]
> For more ways to use the `python-versions` input, please refer to
> ["Using the `python-version` input"] for [`actions/setup-python`].

**Example**:

```yaml
jobs:
  main:
    uses: coatl-dev/workflows/.github/workflows/tox.yml@v7.0.22
    with:
      python-versions: |
        3.9
        3.10
        3.11
        3.12
```

### .github/workflows/uv-pip-compile-upgrade

GitHub action for running [`uv pip compile --upgrade`] on your Python
requirements.

**Inputs**:

- `base-branch` (`string`): The branch to use as the base for the PR.
- `checkout-ref` (`string`): The branch, tag or SHA to checkout. Optional.
- `path` (`string`): The location of the requirement file(s).
- `pr-auto-merge` (`string`): Automatically merge only after necessary
  requirements are met. Options: `'yes'`, `'no'`. Defaults to `'yes'`. Optional.
- `pr-branch` (`string`): The branch to use for the submitting the PR. Defaults
  to `'coatl-dev-pip-compile-upgrade'`. Optional.
- `pr-commit-message` (`string`): Use the given message as the commit message.
  Defaults to `'chore(requirements): pip-compile upgrade'`. Optional.
- `pr-delete-branch` (`string`): Delete the local and remote branch after merge.
  Options: `'yes'`, `'no'`. Defaults to `'no'`. Optional.
- `python-version` (`string`): The version of Python to set `UV_PYTHON` to. You
  may use MAJOR.MINOR or exact version. Options: `'3.8'` to `'3.14'`. Defaults
  to `'3.14'`. Optional.
- `sign-commits` (`string`): Whether to sign Git commits. Options: `'yes'`,
  `'no'`. Defaults to `'yes'`. Optional.
- `working-directory` (`string`): The directory to run the workflow in.
  Optional. Defaults to `github.workspace`.

**Secrets**:

- `gh-token` (`secret`): GitHub token. Required when creating PRs, otherwise is
  optional.
- `gpg-sign-passphrase` (`secret`): GPG private key passphrase. Required when
  signing commits, otherwise is optional.
- `gpg-sign-private-key` (`secret`): GPG private key exported as an ASCII
  armored version. Required when signing commits, otherwise is optional.

**Example**:

```yml
name: uv-pip-compile-upgrade

on:
  schedule:
    - cron: '0 20  * * 1'
  workflow_dispatch:

jobs:
  pip-compile-upgrade:
    uses: coatl-dev/workflows/.github/workflows/uv-pip-compile-upgrade.yml@v7.0.22
    with:
      path: requirements.txt
    secrets:
      gh-token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      gpg-sign-passphrase: ${{ secrets.GPG_PASSPHRASE }}
      gpg-sign-private-key: ${{ secrets.GPG_PRIVATE_KEY }}
```

[`actions/setup-python`]: https://github.com/actions/setup-python
[`coatldev/six`]: https://hub.docker.com/r/coatldev/six
[`env_list`]: https://tox.wiki/en/latest/config.html#env_list
[`ghcr.io/coatl-dev/python-tools`]: https://github.com/coatl-dev/docker-python-tools/pkgs/container/python-tools
[`local hooks`]: https://pre-commit.com/#repository-local-hooks
[`pre-commit`]: https://pre-commit.com/
[`pre-commit autoupdate`]: https://pre-commit.com/#pre-commit-autoupdate
[`pre-commit.ci`]: https://pre-commit.ci/
[Temporarily disabling hooks]: https://pre-commit.com/#temporarily-disabling-hooks
[`tox-gh`]: https://github.com/tox-dev/tox-gh
[testing end-of-life]: https://tox.wiki/en/latest/faq.html#testing-end-of-life-python-versions
["Using the `python-version` input"]: https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#using-the-python-version-input
[`uv pip compile --upgrade`]: https://docs.astral.sh/uv/reference/cli/#uv-pip-compile--upgrade
