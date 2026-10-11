# A main-red fix lands the moment its required checks pass
parent: F-0346

type: story
parent: F-0346

A main-red fix lands the moment its required checks pass. Measured 2026-10-11: main went red at deaf9f1c2 (about 19:40 local), and the kernel opened fix PR #1584 (B-172260). The PR stood green and CLEAN, unmerged, while every merge waited behind red main (no merges for about 50 min). The console merged it by hand at 20:25.

## Acceptance
- A PR from the kernel's main-red fix path is merged (direct merge with --match-head-commit) on the first tick its required checks are green on the exact head; it doesn't wait for a review session (a review can follow after the merge) (test).
- Status shows main-red minutes per day as a flow metric.
