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
      token: ${{ secrets.ANACONDA_TOKEN }}
```
