# Semantic Release Action

Composit GitHub Action that use [semantic-release](https://github.com/semantic-release/semantic-release) to automates the whole package release workflow determining the next version number ad generating the release notes.

## How to use

1. Install following npm packages:

```sh
npm i -D @semantic-release/commit-analyzer @semantic-release/git @semantic-release/github @semantic-release/npm @semantic-release/release-notes-generator conventional-changelog-conventionalcommits
```

Now the Action only installs those packages, so if other @semantic-release extensions are needed, install them before calling the action as shown in the example.
This change has been done to avoid the fail of the release when .npmrc keys are required.

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

      - name: Install @semantic-release/exec
	run: npm install @semantic-release/exec -D

      - name: Semantic Release
        uses: meblabs/semantic-release-action@v1.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}

```
