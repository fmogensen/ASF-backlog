# Ingest keeps a landed Task Active from a stale PR cache although the trunk carries its merge-queue commit

Parent: E-0001
severity: S2

Ingest keeps a landed Task Active because of a stale PR cache, though the trunk already carries its merge. On botseon, T-0485's PR #1046 landed through the merge queue as bc62d6117 ("merge-queue: #1046 (cloud/T-0485 @ 243ce531…)") at about 08:01Z, and main CI was green after it. 40 min later the card still read "PR #1046 OPEN / rule: in-flight", Active, because state/<product>/cache-prs.json (46 min old) still had #1046 as OPEN. The tick's record and PR steps run about every 45 min, so a Feature's next Task (T-0660) waited behind the stale row.

## Acceptance
- A trunk commit that names the item or its branch, including a merge-queue commit whose subject carries `#<pr> (<branch> @ <sha>)`, is landing evidence that outranks a cached PR state of OPEN. The Task moves to Resolved or Closed on that pass. Tested.
- When the merge queue lands a batch, its members' cached PR rows are marked merged in that same step, so no stale OPEN row survives the landing. Tested.
- A cached PR row older than the tick interval is never the only evidence holding an item Active. The ingest re-reads that PR, one host call per such item and capped per pass. Tested.
