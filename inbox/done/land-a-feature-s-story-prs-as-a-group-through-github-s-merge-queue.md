→ S-137105

# Land a Feature's Story PRs as a group through GitHub's merge queue
parent: F-0350

type: story
parent: F-0350

Land a Feature's Story PRs as a group through GitHub's merge queue (operator-approved 2026-10-10; landing half of F-0350).

## Problem
Each PR lands alone: one move of main per PR, at p90 32-36 min (46 min for the v0.2.0 merge), with rebases and re-runs of the PRs behind it. CI itself is short (p50 10 min, p90 15 min); the cost is the moves of main and the untested combination of Stories.

## Change
- Use GitHub's native merge queue: a merge-queue rule in the main ruleset; CI workflows also trigger on `merge_group`; required checks report on merge_group runs.
- The kernel enqueues a Story PR once it is approved and its own required checks are green, and stops direct-merging or auto-merging it. Only individually green+approved PRs are enqueued, so a red group is an interaction failure: one fix round on the Feature naming the grouped PRs, never classified as infra.
- Switch `kernel.landing.merge_queue: true|false` so it can be undone; false reproduces today's landing.
- Measured in F-0350's A/B: moves of main per Feature, rebases and re-runs, minutes from approved to merged, group failures and their cause.

## Acceptance
- With merge_queue on, an approved+green Story PR is enqueued, not direct-merged (test).
- A red merge_group run yields a Feature fix round naming the grouped PRs and is not counted as infra (test).
- merge_queue: false reproduces today's landing (test).
- tests.yml runs the required checks on the merge_group event (test or workflow lint).
