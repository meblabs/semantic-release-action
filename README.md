# Semantic Release Action

Reusable GitHub workflow 

[Semantic-release](https://github.com/semantic-release/semantic-release) automates the whole package release workflow determining the next version number ad generating the release notes.

The repo tha want to use this action need to have npm projects with this package:

```sh
npm i -D @semantic-release/commit-analyzer @semantic-release/git @semantic-release/github @semantic-release/npm @semantic-release/release-notes-generator conventional-changelog-conventionalcommits
```

For config put the `.releaserc` file in main directory.