# record: a date-prefixed plan file mints no Tasks after it lands

A plan file whose name is date-prefixed and does not contain the item id
(e.g. docs/superpowers/plans/2026-09-20-free-plan.md for F-0019, landed from cloud/plan-F-0019)
mints no Task cards after it lands; a plan named f-0079.md for F-0079 minted all 14.

Expected: the plan → Feature link resolves from the lane branch / PR that landed it (cloud/plan-<ITEM>)
or the plan's own header, not only from the file name; the plan brief can also name the file
(`{plan_path}`) but existing plans with dated names must still mint.
Acceptance: record test — a dated plan landed from cloud/plan-F-xxxx mints its `### Task N:` cards.
