# Dependabot Auto Merge

A GitHub Action that automatically enables auto-merge on open Dependabot PRs using the GitHub CLI.

## Usage

```yaml
name: Auto-merge Dependabot PRs

on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours
  workflow_dispatch:       # Manual trigger

jobs:
  auto-merge:
    runs-on: ubuntu-latest
    steps:
      - name: Auto-merge Dependabot PRs
        uses: gesslar/dependabot-auto-comment-merge@main
        with:
          # GitHub token with permissions to merge PRs
          # For organization-wide access, use a personal access token
          token: ${{ secrets.GITHUB_TOKEN }}

          # Owner of the repositories (username or organization)
          # Default: current repository owner
          repo-owner: ${{ github.repository_owner }}

          # Comma-separated list of repositories to check for Dependabot PRs
          # If not set, all repositories of the owner will be processed
          # Example: "repo1,repo2,repo3"
          repositories: ${{ vars.DEPENDABOT_AUTO_COMMENT_REPOS }}

          # Comma-separated list of repositories to ignore
          # Useful when processing all repos but wanting to exclude specific ones
          # Example: "ignore-repo1,ignore-repo2"
          ignore-repositories: ${{ vars.DEPENDABOT_AUTO_COMMENT_IGNORE_REPOS }}

          # Dry run mode - if true, will only log actions without posting comments
          # Default: false
          dry-run: ${{ vars.DEPENDABOT_DRY_RUN }}

          # Merge type - controls which merge strategy is used: merge, rebase, or squash
          # Default: merge
          merge-type: merge
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `token` | GitHub token with permissions to merge PRs (for example, `contents: write` and `pull-requests: write`). For accessing repositories across an organization, use a personal access token with appropriate permissions. | Yes | N/A |
| `repo-owner` | Repository owner (can be a username or organization name). Used to determine which repositories to scan for Dependabot PRs. | No | `${{ github.repository_owner }}` |
| `repositories` | Comma-separated list of repositories to check for Dependabot PRs. If not provided, all repositories of the owner will be checked. | No | All repos of owner |
| `ignore-repositories` | Comma-separated list of repositories to ignore. Useful when checking all repos but wanting to exclude a few. | No | None |
| `dry-run` | If set to "true", the action will log what it would do without actually merging. Useful for testing. | No | `false` |
| `merge-type` | Specifies the merge strategy to use when enabling auto-merge for Dependabot PRs. Valid values are: `merge`, `rebase`, or `squash`. | No | `merge` |

## License

`dependabot-auto-comment-merge` is released under the [0BSD](LICENSE.txt).

This package includes or depends on third-party components under their own
licenses:

| Dependency | License |
| --- | --- |
| [@actions/core](https://github.com/actions/toolkit) | MIT |
| [@actions/github](https://github.com/actions/toolkit) | MIT |
