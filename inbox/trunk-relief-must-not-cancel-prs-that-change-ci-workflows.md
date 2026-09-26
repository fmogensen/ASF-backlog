# Trunk relief must not cancel PRs that change CI workflows

Relief cancelled botseon PR #849 run 36245122391 (p1-e2e, m8-e2e, site, deploy-dev). That PR changes .github/workflows to reserve runners for main, the very change that makes relief unnecessary. It was re-run by hand at 16:04.

Expected: trunk relief never cancels a PR run whose PR changes CI config (.github/workflows/**, or configurable `ci.queue.relief_exempt_paths`). Nor does it cancel runs of PRs labelled or classified as infra/ci. It logs "exempt: CI change".

Test: a PR touching .github/workflows/ci.yml is never a relief candidate.
