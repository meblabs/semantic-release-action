# Semantic Release Action

Composit GitHub Action that use [semantic-release](https://github.com/semantic-release/semantic-release) to automates the whole package release workflow determining the next version number ad generating the release notes.

## How to use

1. Install following npm packages:

```sh
npm i -D @semantic-release/commit-analyzer @semantic-release/git @semantic-release/github @semantic-release/npm @semantic-release/release-notes-generator conventional-changelog-conventionalcommits
```

2. Put config file `.releaserc` in main directory and if necessary customize it.

3. Create workflow in your repo:

```yml
name: Release

on:
  push:
    branches: [release, staging]

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Semantic Release
        uses: meblabs/semantic-release-action@v1.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

```
