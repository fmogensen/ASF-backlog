→ B-0107

# feeder: a pushed plan/spec awaiting its PR is relaunched as STARVED → PLAN/SPEC
signature: feeder lists STARVED to PLAN for a Feature whose plan branch is pushed with an open PR, and a second plan session is launched
parent: E-0001
severity: S2

In the pull-request landing mode, a plan (or spec) session that finished and pushed its branch leaves
the Feature at plan-draft until its PR merges. The feeder then lists it as STARVED → PLAN ("a plan in
draft/review that no session is moving") and the next free slot relaunches a second plan session for
a document that is already written and only waiting on its PR.

Seen: a product's status showed "Ready to launch: 4 — first: STARVED → PLAN <feature>" while
cloud/plan-<feature> was pushed with a REPORT `status: done, pushed: yes`.

Expected: a spec/plan whose lane branch is pushed with an open PR (or a done report awaiting
harvest) is not STARVED — the row waits on the PR. Only an unmoved draft with no pushed lane
branch is starved.
Acceptance: a feeder test where the plan branch exists with an open lane PR yields no STARVED → PLAN row.
