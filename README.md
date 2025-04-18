# Build and Publish Anaconda Package
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Github workflow to build your software package and publish to an Anaconda repository.

### Example workflow to build and publish to anaconda every time you make a new release

This example builds your application on multiple plateforms, with multiple python versions. This is a template for your `.github/workflow/build_publish_anaconda.yml`.


```yaml


name: Building Package

on:
  push:
    branches:
      - '**'
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
```

Arguments to this GH-actionare set to default values, which are :

```yaml
  conda_directory: conda
  python_minor_version: [8, 9, 10, 11, 12]
  operating_system: [ubuntu-latest, macos-latest, macos-13, windows-latest]
  numpy-version: 0
  conda-channels: ${{ vars.ANACONDA_CHANNELS}}
  build-options: "--no-test"
```

but you can override them in the workflow file.

For exemple, if you are in a `dev-new-feature` branch, and you want to run the tests on `ubuntu-latest` and `macos-latest`, `python 3.10` only, then your workflow file would look like this:

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
      python_minor_version: [ 10 ]
      operating_system: [ ubuntu-latest, macos-latest ]
      build-options: ""
```