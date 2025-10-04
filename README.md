# Semantic Release Action

Composite GitHub Action that uses [semantic-release](https://github.com/semantic-release/semantic-release) to:
- calculate the next version based on conventional commits
- generate release notes
- publish tags and GitHub Releases
- **optional**: synchronize `release → staging` to ensure prereleases are based on the latest stable

## Requirements for the repository using this action

#### 1 install dependeces
```sh
npm i -D @semantic-release/commit-analyzer @semantic-release/git @semantic-release/github @semantic-release/npm @semantic-release/release-notes-generator conventional-changelog-conventionalcommits
```

#### 1 config file
Place the `.releaserc` config file in the main directory and customize it if needed.

## Action Inputs

| input | default | description |
|---|---|---|
| `token` | — | `GITHUB_TOKEN` with `contents:write` |
| `checkout` | `true` | If `true`, the action checks out the repo |
| `node-version` | `22.x` | Node version for the jobs |
| `sync` | `true` | If enabled, after release on `release` branch, syncs **release → staging** |
| `bot_name` | `MeblabsBot` | Author name for sync commits |
| `bot_email` | `github@meblabs.com` | Author email for sync commits |
| `sync_commit_message` | `chore: history sync release -> staging (ours)` | Commit message for sync (merge `-s ours`) |

## Usage

**Option 1 · Checkout handled by the action**
```yml
- name: Semantic Release
  uses: meblabs/semantic-release-action@v2
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    checkout: true
    node-version: 22.x           # optional
    sync: true                   # default: true
```

**Option 2 · Explicit checkout in the workflow**
```yml
- name: Checkout
  uses: actions/checkout@v4
  with:
    fetch-depth: 0              
    fetch-tags: true            

- name: Semantic Release
  uses: meblabs/semantic-release-action@v2
  with:
    token: ${{ secrets.GITHUB_TOKEN }}
    checkout: false
    sync: true
```

## How the `release → staging` sync works

The sync step runs **only when the workflow runs on `release`** and `sync: true`.

1. Targeted fetch of `release`, `staging`, and tags.
2. Checkout of updated `staging`.
3. Attempts a **fast‑forward** from `release`. If possible, no commit is made.
4. If not possible, performs a **“history‑only” merge**:  
   `git merge -s ours --no-ff origin/release -m "<sync_commit_message>"`

Effect:
- Does not change files in `staging`.
- Imports **only the ancestry** of `release`. Stable tags become **reachable** from `staging`.
- Future prereleases will be like `X.Y.Z-staging.N` where `X.Y.Z` is the latest stable.

## Notes

- If you manage releases with separate PRs `dev → staging` and `dev → release`, the sync avoids long-term history divergence and keeps prereleases consistent.
- If you don't want sync, set `sync: false` and ensure yourself that `staging` contains the latest `release`.
- For npm publishing, also add `@semantic-release/npm` and related configuration.