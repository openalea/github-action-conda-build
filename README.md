# Build and Publish Anaconda Package
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A reusable Github workflow to build your software package and publish to an Anaconda repository following openalea guidelines.

## Usage

### Build and publish according to openalea guidelines

Copy the template below in your source dir, using the following path: `.github/workflow/build_publish_anaconda.yml`.


```yaml
# your_package/.github/workflow/build_publish_anaconda.yml

name: Building Package after Openalea guidelines

on:
  push:
    branches:
      - '**'
    tags:
      - 'v*'
  pull_request:
    branches:
      - '**'
  release:
    types: [published]

# avoid duplicating jobs
concurrency:
  group: ${{ github.workflow }}-${{ github.head_ref || github.ref }}
  cancel-in-progress: true

jobs:
  build:
    uses: openalea/github-action-conda-build/.github/workflows/conda-package-build.yml@main
    secrets:
      anaconda_token: ${{ secrets.ANACONDA_TOKEN }}
```
Note that to publish your package to your anaconda channel, you must meet one of the two following conditions :

- have a tag that defines a new version of your package : `v...`. This allows uploading a new version of the package on the `main` anaconda channel
- be on the `main` / `master` branch and define a label `test` or `dev` or `latest` (the former is recommanded). For exemple, you want to release your package on the `latest` channel of anaconda, only one version of python but on all os :

### Build and publish with different options 

If you do not follow completely (and/or debug) guidelines, you may need to adjust some options to non default values.
Main options (with current defaults) are:

```yaml
  conda_directory: conda
  python-minor-version: [9, 10, 11, 12]
  operating-system: "['ubuntu-latest', 'macos-latest', 'macos-13', 'windows-latest']"
  numpy-version: 0
  conda-channels: 'conda-forge,openalea3'
  build-options: ""
```

You can override them in the workflow file, by adding a 'with' section at the end of the file.

For example, if you want to run without launching test on `ubuntu-latest` and `macos-latest`, `python 3.10` only, then your workflow file would look like this:

```yaml


name: build_publish_anaconda

...

jobs:
  build:
    uses: openalea/github-action-conda-build/.github/workflows/conda-package-build.yml@main
    secrets:
      anaconda_token: ${{ secrets.ANACONDA_TOKEN }}
    with:
      python-minor-version: "[ 10 ]"
      operating-system: '["ubuntu-latest", "macos-latest"]'
      build-options: "--no-test"
```

### Use the reusable workflow in a different context 

Many of the rules defined in this workflow can be controled by options, so that you can implement/test  build/publish strategies far from openalea guidelines.

For example, if you are in a `dev-new-feature` branch, and you want to run the tests on `ubuntu-latest` and `macos-latest`, `python 3.10` only, then your workflow file would look like this:

```yaml


name: Building Package

on:
  push:
    branches:
      - dev-new-feature
    tags:
      - 'v*'
  pull_request:
    branches:
      - '**'


jobs:
  build:
    uses: openalea/github-action-conda-build/.github/workflows/conda-package-build.yml@main
    secrets:
      anaconda_token: ${{ secrets.ANACONDA_TOKEN }}
    with:
      python-minor-version: "[ 10 ]"
      operating-system: '["ubuntu-latest", "macos-latest"]'
      build-options: ""
```

