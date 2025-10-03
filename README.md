# Semantic Release Action

Composit GitHub Action that use [semantic-release](https://github.com/semantic-release/semantic-release) to automates the whole package release workflow determining the next version number ad generating the release notes.

## How to use

#### 1. Install following npm packages:

```sh
npm i -D @semantic-release/commit-analyzer @semantic-release/git @semantic-release/github @semantic-release/npm @semantic-release/release-notes-generator conventional-changelog-conventionalcommits
```

#### 2. Config file

Put config file `.releaserc`  in main directory and if necessary customize it.

#### 3. Create workflow in your repo:

You can delegate the repository checkout step to the action itself, or handle it explicitly in your workflow. 

**Option 1: Checkout handled by the action**

The action will automatically checkout your repository before running semantic-release. Set `checkout: true` in the action configuration:

```yml
- name: Semantic Release
  uses: meblabs/semantic-release-action@v2
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    checkout: true
```

**Option 2: Explicit checkout in your workflow**

If you need more control (e.g., fetching tags), perform the checkout step manually and set `checkout: false` in the action configuration:

```yml
- name: Checkout
  uses: actions/checkout@v4
  with:
    fetch-depth: 0

- name: Ensure tags
  shell: bash
  run: git fetch --tags --force

- name: Semantic Release
  uses: meblabs/semantic-release-action@v2
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    checkout: false
```

