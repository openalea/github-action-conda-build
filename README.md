# Build and Publish Anaconda Package
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A Github workflow to build your software package and publish to an Anaconda repository following openalea guidelines.

### Example workflow to build and publish to anaconda according to openalea guidelines

To operate, you just have to copy this template in your `.github/workflow/build_publish_anaconda.yml`.


```yaml


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

Arguments to this GH-action are set to default values, which are :

```yaml
  conda_directory: conda
  python-minor-version: [9, 10, 11, 12]
  operating-system: "['ubuntu-latest', 'macos-latest', 'macos-13', 'windows-latest']"
  numpy-version: 0
  conda-channels: ${{ vars.ANACONDA_CHANNELS}}
  build-options: ""
```

You can override them in the workflow file.

For example, if you want to run without test on `ubuntu-latest` and `macos-latest`, `python 3.10` only, then your workflow file would look like this:

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

You can also define a workflow that pilot the action in totally different way from openalea guidelines.
Eg the following workflows build and publish on label 'latest' all tags starting with 'v' for one python version only:
``yaml
name: Building Package with specilised rules

on:
  push:
    tags:
      - 'v*'

jobs:
  build:
    uses: openalea/github-action-conda-build/.github/workflows/conda-package-build.yml@main
    secrets:
      anaconda_token: ${{ secrets.ANACONDA_TOKEN }}
    with:
      python-minor-version: "[ 10 ]"
      label: "latest"
	  suffix: ""
```
