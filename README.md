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

Arguments to this GH-action are set to default values, which are :

```yaml
  conda_directory: conda
  python-minor-version: [8, 9, 10, 11, 12]
  operating-system: "['ubuntu-latest', 'macos-latest', 'macos-13', 'windows-latest']"
  numpy-version: 0
  conda-channels: ${{ vars.ANACONDA_CHANNELS}}
  build-options: "--no-test"
  label: main
```

but you can override them in the workflow file.

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
      python-minor-version: [ 10 ]
      operating-system: '["ubuntu-latest", "macos-latest"]'
      build-options: ""
```

Also, note that to publish your package to your anaconda channel, you must meet one of the two following conditions :

- have a tag that defines a new version of your package : `v...`. This allows uploading a new version of the package on the `main` anaconda channel
- be on the `main` / `master` branch and define a label `test` or `dev` or `latest` (the former is recommanded). For exemple, you want to release your package on the `latest` channel of anaconda, only one version of python but on all os : 
```yaml
name: Building Package

...

jobs:
  build:
    uses: openalea/github-action-conda-build/.github/workflows/conda-package-build.yml@main
    secrets:
      anaconda_token: ${{ secrets.ANACONDA_TOKEN }}
    with:
      python-minor-version: [ 10 ]
      label: latest
```
