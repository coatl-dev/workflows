## v7.0.19 (2026-07-20)

### Refactor

- **deps**: update actions/checkout action to v7.0.1 (#158)

## v7.0.18 (2026-07-20)

### Refactor

- **deps**: update actions/setup-python action to v7 (#154)

## v7.0.17 (2026-07-17)

### Refactor

- update coatl-dev/actions action to v7.0.10 (#153)

## v7.0.16 (2026-07-10)

### Refactor

- **deps**: update docker/setup-buildx-action action to v4.2.0 (#151)

## v7.0.15 (2026-07-10)

### Refactor

- **deps**: update docker/metadata-action action to v6.2.0 (#147)

## v7.0.14 (2026-07-10)

### Refactor

- **deps**: update docker/login-action action to v4.4.0 (#146)

## v7.0.13 (2026-07-10)

### Refactor

- **deps**: update docker/build-push-action action to v7.3.0 (#145)

## v7.0.12 (2026-07-10)

### Refactor

- **deps**: update coatl-dev/actions action to v7.0.9 (#144)

## v7.0.11 (2026-07-06)

### Refactor

- **deps**: bump coatl-dev/actions/pr-create from 7.0.4 to 7.0.5 (#135)

## v7.0.10 (2026-07-06)

### Refactor

- **deps**: bump actions/setup-python from 6.2.0 to 6.3.0 (#134)

## v7.0.9 (2026-07-06)

### Refactor

- **deps**: bump coatl-dev/actions/pypi-upload from 7.0.4 to 7.0.5 (#136)

## v7.0.8 (2026-07-06)

### Refactor

- **deps**: bump actions/cache from 5.0.5 to 6.1.0 (#133)

## v7.0.7 (2026-07-06)

### Refactor

- **deps**: bump coatl-dev/actions/simple-git-diff (#132)

## v7.0.6 (2026-06-26)

### Fix

- **docker-build**: fix parsing error (#131)

## v7.0.5 (2026-06-26)

### Fix

- **docker-build**: resolve shell expansion issue (#130)

## v7.0.4 (2026-06-26)

### Refactor

- apply zizmor recommendations (#129)

## v7.0.3 (2026-06-22)

### Refactor

- **deps**: bump coatl-dev/actions from 7.0.0 to 7.0.1 (#127)

## v7.0.2 (2026-06-22)

### Refactor

- **deps**: bump actions/checkout from 6.0.3 to 7.0.0 (#126)

## v7.0.1 (2026-06-15)

### Refactor

- **deps**: bump actions/checkout (#124)

## v7.0.0 (2026-06-09)

### BREAKING CHANGE

- drop pr-create input; a PR will always be created

### Feat

- add prek and prek-autoupdate workflows (#123)

### Refactor

- use coatl-dev/actions@v7.0.0 (#122)
- switch to hash-pinned actions (#121)

## v6.2.5 (2026-03-09)

### Refactor

- **deps**: bump docker/build-push-action from 6 to 7 (#118)
- **deps**: bump docker/login-action from 3 to 4 (#117)
- **deps**: bump docker/setup-buildx-action from 3 to 4 (#116)
- **deps**: bump docker/metadata-action from 5 to 6 (#115)
- **deps**: bump actions/upload-artifact from 6 to 7 (#113)
- **deps**: bump actions/download-artifact from 7 to 8 (#112)

## v6.2.4 (2025-12-30)

### Refactor

- add pr-branch input (#109)

## v6.2.3 (2025-12-15)

### Refactor

- **deps**: bump actions/upload-artifact from 5 to 6 (#107)
- **deps**: bump actions/download-artifact from 6 to 7 (#106)
- **deps**: bump actions/cache from 4 to 5 (#105)

## v6.2.2 (2025-12-11)

### Refactor

- add pr-create-additional-args input (#104)

## v6.2.1 (2025-12-10)

### Refactor

- add checkout-ref input (#103)

## v6.2.0 (2025-11-28)

### Feat

- use Python 3.14 as default (#102)

### Refactor

- **deps**: bump actions/checkout from 5 to 6 (#101)
- **deps**: bump actions/download-artifact from 5 to 6 (#99)
- **deps**: bump actions/upload-artifact from 4 to 5 (#98)

## v6.1.4 (2025-10-12)

### Refactor

- **tox-docker**: set all caching variables in one step (#96)

## v6.1.3 (2025-10-11)

### Refactor

- **tox-docker**: run tox with uvx (#95)

## v6.1.2 (2025-10-10)

### Refactor

- **tox-docker**: add $HOME/.local/bin to PATH before installing tox (#94)

## v6.1.1 (2025-10-10)

### Fix

- **tox-docker**: add $HOME/.local/bin to $PATH (#93)

## v6.1.0 (2025-10-09)

### Feat

- **tox-docker**: use uv for running tox (#92)

## v6.0.5 (2025-09-08)

### Refactor

- **deps**: bump actions/setup-python from 5 to 6 (#90)

## v6.0.4 (2025-08-12)

### Refactor

- **deps**: bump actions/checkout from 4 to 5 (#88)
- **deps**: bump actions/download-artifact from 4 to 5 (#86)

## v6.0.3 (2025-07-01)

### Refactor

- **pypi-upload**: use coatl-dev/actions/pypi-upload (#84)

## v6.0.2 (2025-06-30)

### Refactor

- **deps**: bump coatl-dev/actions from 4 to 5 (#83)

## v6.0.1 (2025-06-28)

### Refactor

- **pypi-upload**: use Python 2.7 as the default value (#82)

## v6.0.0 (2025-06-27)

### BREAKING CHANGE

- pip-copile-upgrade is now meant for Python 2.7 only

### Refactor

- **pip-compile-upgrade**: use ghcr.io/coatl-dev/python-tools (#81)

## v5.0.7 (2025-06-27)

### Refactor

- **pypi-upload**: run on ghcr.io/coatl-dev/python-tools (#80)

## v5.0.6 (2025-06-21)

### Fix

- **tox**: fix tox cache and hashFiles paths (#78)

## v5.0.5 (2025-06-17)

### Fix

- **pypi-upload**: set working-directory for build step (#76)

## v5.0.4 (2025-06-17)

### Refactor

- add working-directory input to tox-[docker|gh] (#75)

## v5.0.3 (2025-06-17)

### Refactor

- **pylint**: add extra-args and working-directory inputs (#74)

## v5.0.2 (2025-06-17)

### Refactor

- add working-directory input to… (#73)

## v5.0.1 (2025-06-17)

### Refactor

- **tox**: add extra-args input for tox and tox-docker (#72)

## v5.0.0 (2025-06-03)

### BREAKING CHANGE

- remove tox-envs in favor of tox

### Refactor

- merge tox-envs into tox (#71)

## v4.3.2 (2025-05-07)

### Refactor

- **pip-compile-upgrade**: add extra-args input (#70)

## v4.3.1 (2025-05-05)

### Refactor

- use coatl-dev/actions@v4 (#69)

## v4.3.0 (2025-05-02)

### Feat

- add uv-pip-compile-upgrade (#68)

## v4.2.5 (2025-02-27)

## v4.2.4 (2025-02-01)

### Fix

- **docker-build**: fix build-cache validation (#61)

## v4.2.3 (2025-02-01)

### Refactor

- **docker-build**: add steps for cached and non-cached builds (#60)

## v4.2.2 (2025-01-31)

### Refactor

- **docker-build**: add ability to disable cache (#59)

## v4.2.1 (2025-01-28)

### Refactor

- **pylint**: add path input (#58)

## v4.2.0 (2025-01-27)

### Feat

- add docker-build-push-multi-registry (#57)

## v4.1.7 (2025-01-26)

### Fix

- **docker-build**: remove set up qemu action (#56)

## v4.1.6 (2025-01-17)

### Refactor

- **docker-build**: build linux/arm64 on ubuntu-24.04-arm runner (#55)

## v4.1.5 (2025-01-17)

### Refactor

- **docker-build**: build linux/arm64 on ubuntu-24.04-arm runner (#55)

## v4.1.4 (2024-10-23)

### Refactor

- default to Python 3.13 (#51)

## v4.1.3 (2024-10-07)

### Refactor

- drop dependency on base.txt (#48)
- use ubuntu-latest (#47)

## v4.1.2 (2024-09-10)

### Refactor

- set pr-delete-branch default to 'no' (#45)

## v4.1.1 (2024-09-09)

### Refactor

- add pr-delete-branch input (#44)

## v4.1.0 (2024-09-01)

### Feat

- add docker-build-push-multi-platform (#43)

## v4.0.0 (2024-06-18)

### BREAKING CHANGE

- drop python-version input

### Refactor

- **tox-docker**: use coatldev/six:latest container (#31)

## v3.0.2 (2024-05-20)

### Refactor

- **tox-docker**: fix python-version input description (#28)

## v3.0.1 (2024-03-06)

### Refactor

- use coatl-dev/actions@v3 (#24)

## v3.0.0 (2024-02-21)

### BREAKING CHANGE

- replace Python 2.7 and 3.11 with 3.12 as default

### Refactor

- use Python 3.12 as default version (#23)

## v2.1.2 (2024-01-21)

### Refactor

- **deps**: bump actions/cache from 3 to 4 (#20)

## v2.1.1 (2024-01-09)

### Refactor

- use coatl-dev/actions@v2 (#19)

## v2.1.0 (2024-01-09)

### Feat

- add pip-compile-upgrade and pre-commit-autoupdate (#18)

## v2.0.3 (2023-12-11)

## v2.0.2 (2023-11-21)

### Refactor

- use coatl-dev/workflow-requirements base requirements (#14)

## v2.0.1 (2023-11-17)

### Refactor

- use new requirements path (#13)

## v2.0.0 (2023-11-16)

### BREAKING CHANGE

- drop unused inputs and set Python 3.11 and ubuntu-22.04 as default
- drop black, flake8 and mypy

### Refactor

- discard unused inputs (#11)
- drop unused workflows (#10)

### Perf

- use coatl-dev/workflow-requirements to improve caching (#12)

## v1.1.2 (2023-10-29)

### Refactor

- modify Python package install strategy (#7)

## v1.1.1 (2023-10-28)

### Refactor

- **pypi-upload**: use official Python Docker image (#6)

## v1.1.0 (2023-10-12)

### Feat

- **workflows**: make caching optional (#3)

## v1.0.0 (2023-10-08)
