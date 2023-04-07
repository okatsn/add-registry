# Add Registry
`add-registry` is a github actions that simply add personal julia registry.

## Requirement

- The registry should be hosted under `"https://github.com/"`. (e.g., "https://github.com/okatsn/OkRegistry.git").

- The registry has to be public.

## Input
`author-name`: The 'author/repository-name'. Default is 'okatsn/OkRegistry'.

## Usage
Example for generating julia Documentation of a package named `SWCForecast` which lives under https://github.com/okatsn/OkRegistry.git:
```yml
name: CI
on:
  push:
    branches:
      - main
    tags: ['*']
  pull_request:

jobs:
  docs:
    name: Documentation
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
      - uses: julia-actions/setup-julia@v1
        with:
          version: '1'
      - uses: okatsn/add-registry@v2
        with:
          author-name: 'okatsn/OkRegistry'

      - uses: julia-actions/julia-buildpkg@v1
      - uses: julia-actions/julia-docdeploy@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - run: |
          julia --project=docs -e '
            using Documenter: DocMeta, doctest
            using SWCForecast
            DocMeta.setdocmeta!(SWCForecast, :DocTestSetup, :(using SWCForecast); recursive=true)
            doctest(SWCForecast)'
```

