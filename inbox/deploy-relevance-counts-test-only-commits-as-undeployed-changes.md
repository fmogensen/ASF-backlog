# Deploy relevance counts test-only commits as undeployed changes

A product's site deploy row said "1 relevant commits behind main … waits on a hand deploy" for a commit that only changed apps/site/playwright.config.ts, a test-runner config. The deployed site was byte-identical. Every test-only edit reads as an undeployed change.

Wanted in code (asf/harvest/deploy.py, the relevance path filter for named targets, prod and dev):
- Default exclude globs: **/*.test.*, **/*.spec.*, **/e2e/**, **/playwright.config.*, **/vitest.config.*, **/__tests__/**, **/*.md.
- A per-target `exclude:` list under deploy_sha.<env>/targets.<name>, added to the defaults, with an `exclude_defaults: false` switch.
- Relevance counts only commits touching a path under the target's paths and outside its excludes.
- Tests for both.
