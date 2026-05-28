# GitHub Stats

This repository stores the public-safe stats data for my GitHub profile. It is
powered by [`LukasParke/stats-action`](https://github.com/LukasParke/stats-action)
and is intended to feed profile README widgets, a live interactive stats site,
and Remotion-generated animations.

## What Gets Published

The workflow commits:

- `github-user-stats.json`: generated stats output with the v2 schema plus
  legacy aliases for older consumers.
- `.github-profile-stats/cache.json`: stable cache used to avoid repeatedly
  refetching historical data and to resume optional backfill.

The workflow does not commit `.github-profile-stats/volatile-cache.json`; that
file is restored and saved through GitHub Actions cache.

## Privacy Model

This repo may be public, so private repository details are disabled in the
workflow:

- `include-private-repository-details: false`
- `include-private-cache-details: false`

Aggregate private activity values may still appear, such as private repository
count, restricted contribution count, and total contribution counts. Identifying
private repository data should not be committed: names, descriptions, URLs,
topics, branch identifiers, per-repository traffic, or per-repository
contributor stats.

## Workflow Setup

The scheduled workflow uses a Personal Access Token stored as `ACCESS_TOKEN`.
For a fresh setup:

1. Create a PAT for the GitHub account being measured.
2. Give it read access for profile and repository data.
3. Add it to this repository as a secret named `ACCESS_TOKEN`.
4. Run **GitHub Profile Stats** manually from the Actions tab.

The first run may not complete every optional repository metric. Later scheduled
runs resume the backfill queue using `.github-profile-stats/cache.json`.

## Useful Output Sections

- `profile`: public profile fields and social counts.
- `profileContributions`: contribution graph totals, calendar, streaks, and rollups.
- `repositories`: public repository metadata only.
- `repoMetrics`: public-safe repository aggregates and optional metrics.
- `presentation.readmeSummary`: compact values for README/profile widgets.
- `presentation.remotion`: scene-ready summary data for animation generation.
- `privacy`: redaction status and counts.

## Local Checks

```bash
jq '.schemaVersion, .privacy, .collectionStatus.complete' github-user-stats.json
```
