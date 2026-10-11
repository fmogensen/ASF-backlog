→ S-176963

# ASF's own brief templates go through kernel review, not a person (amendable_review + asf propose)
parent: F-0356

type: story
parent: F-0356
severity: S1

ASF's own brief templates go through review, not a person. Measured 2026-10-11: two Tasks in one day (T-81134, T-164021) were blocked by the `touch_amendable_set` hook because their `writes:` named files under asf/briefs/templates/. That path is in the amendable set (asf/amendable.py derives role_agents from TEMPLATES_DIR), so only a person may change it, and `asf propose` doesn't exist. The console hand-applied one edit; the other Task shipped with two acceptance lines marked Not proved.

## Change
- A product may declare in config which amendable paths are kernel-reviewable: `conventions.amendable_review: [asf/briefs/templates/*]`. A session may then edit them on its branch when its card's `writes:` names them. The PR gets a mandatory strong-model review with a diff-of-instructions checklist (what agents will now be told differently), and the merge stays blocked until that review approves.
- Paths not listed keep today's person-only rule (CONSTITUTION, rules).
- Implement `asf propose --from-hold <item>/<hold> --why …` so a held session records a proposal the kernel routes to that review, never to a person by default.

## Acceptance
- With templates listed in amendable_review, a session's template edit on its branch is accepted by the hook, and its PR can't merge without an approving strong-model review (test).
- With the list empty, today's behaviour holds (test).
- `asf propose` records the proposal, and the kernel launches the review (test).
