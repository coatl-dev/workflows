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
