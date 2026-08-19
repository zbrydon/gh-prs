# gh-prs

A GitHub CLI extension for viewing open pull requests as a compact, colour-coded, age-sorted table.

## Install

```sh
gh extension install zbrydon/gh-prs
```

## Usage

### Your own PRs

View open pull requests you have created (shows review decision status):

```sh
gh prs
gh prs --drafts
```

### PRs in your teams' repos

View open PRs across the repositories owned by your teams (excludes your own PRs and bots like Renovate/Dependabot):

```sh
gh prs --req
gh prs --req --drafts
```

Team repos are discovered via the `owner-<team-slug>` repository topic.

### Renovate / Dependabot PRs

By default the team views show only human-raised PRs. Use `--renovate` (alias `--bots`) to swap to showing **only** Renovate/Dependabot PRs instead:

```sh
gh prs --renovate            # bot PRs in your teams' repos (implies --req)
gh prs --rr --renovate       # bot PRs requesting your review
gh prs --renovate --drafts   # draft bot PRs
```

### PRs requesting your review

View PRs where a review has been requested from you, either directly or through any team you belong to. This spans **all repositories** on GitHub, not just those owned by your teams. It also keeps showing any still-open PR you have **already reviewed** — submitting a review (especially on a team's behalf) clears the pending request, so these would otherwise vanish; they remain listed until the PR merges or closes:

```sh
gh prs --rr
gh prs --review-requested
gh prs --rr --drafts
```

### Scoping to specific teams

Limit `--req`/`--rr` to a comma-separated list of `org/team-slug` teams instead of all your teams (passing `--teams` on its own implies `--req`):

```sh
gh prs --teams my-org/backend,my-org/platform
gh prs --rr --teams my-org/backend
```

## Options

| Option | Description |
| --- | --- |
| `--drafts` | Show **only** draft PRs (default hides drafts) |
| `--req` | Show open PRs in your teams' repos (default: your own PRs) |
| `--rr`, `--review-requested` | Show PRs where review is requested from you (directly or via any team), across all repos, plus still-open PRs you have already reviewed |
| `--renovate`, `--bots` | In team views, show **only** Renovate/Dependabot PRs instead of human-raised ones (default: human-raised only). Implies `--req` unless `--rr` is also given |
| `--teams <list>` | Comma-separated `org/team-slug` list to scope `--req`/`--rr` to (default: your own teams). Implies `--req` unless `--rr` is also given |
| `-h`, `--help` | Show help |

`--req` and `--rr` are mutually exclusive.

## Output

PRs are rendered as an aligned table sorted with the longest-open first, including an age column and a clickable URL. Bot-authored PRs (Renovate/Dependabot) are excluded from the team views by default; pass `--renovate` to show only those instead.

## Requirements

- [GitHub CLI (`gh`)](https://cli.github.com/), authenticated (`gh auth login`)
- [`jq`](https://stedolan.github.io/jq/)
