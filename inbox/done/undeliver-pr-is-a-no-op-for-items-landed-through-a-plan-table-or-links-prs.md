→ B-213443

# undeliver --pr is a no-op for items landed through a plan table or links.prs
parent: E-0006

Type: Bug

`asf undeliver <item> --pr N` is a silent no-op for an item that landed through a plan's task table or through `links.prs` (follow-up to B-205135, PR #1713, merged after four review rounds with this finding open).

asf/record/ingest.py `_exclude_pr_ids` (about line 744) only rewrites `ev['ids']`. The excluded set is not carried to the plan-matched path (about lines 1160-1163: `merged` is still set when the task's `pr` is excluded, or when the merged sha belongs to an excluded PR) nor to `match_bug` (about lines 403 and 1259: an excluded PR still sets `merged_sha` and `has_fixer`). Use the same int-keyed sha coercion as line 732.

## Acceptance
- [ ] A Bug with `links: {prs: [N]}` and `excluded_prs: [N]` stays New.
- [ ] A plan-matched Task whose task-table `pr` is N, with N excluded, stays New.
- [ ] An item whose merged sha belongs to an excluded PR is not read as merged.
- [ ] `asf undeliver <item> --pr N` on each of the three shapes changes the item's state, or says why it did not.
