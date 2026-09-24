→ B-0114

# native landing: a merged spec/plan PR marks its Feature landed/Resolved, so no coder ever launches
signature: a merged plan PR leaves the Feature at stage=landed state=Resolved, so its minted Tasks stay New and no coder ever launches
parent: E-0002
severity: S2

With native PR landing in pull-request mode, harvest squash-merges a spec/plan PR. On the next record
step the Feature reads stage=landed, state=Resolved — as if its code had shipped. Seen on 8 Features
at once (6 plans, 2 specs). Squash subjects on the trunk looked like `docs(plan): F-0047 — …`,
`docs(spec): F-0129 — …`, `plan(OPS-1): the F-0037 plan …`, `plan(F-0019): …`.

Effect: the feeder launches PLAN → CODE only at plan-approved/building, so a Feature whose plan just
landed never gets a coder — its minted Tasks sit New forever.

Expected: a merged lane PR of kind spec/plan (branch spec-/plan- prefix, or only specs_dir/plans_dir/
reviews_dir paths) makes the Feature spec-approved / plan-approved, never landed/Resolved — whatever
the squash subject says. Native landing should also write the squash subject in the doc-lane form
(`plan(<ITEM>): …`) so DOC_LANE_SUBJECT recognises it.
Acceptance: an ingest test where a plan PR squash-merges with a non-conforming subject leaves the
Feature at plan-approved with its Tasks New and launchable.
