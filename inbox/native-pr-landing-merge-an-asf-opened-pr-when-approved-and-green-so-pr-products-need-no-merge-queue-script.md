# Native PR landing: merge an ASF-opened PR when approved and green, so PR products need no merge-queue script

A product that lands through pull requests (conventions.landing: pull-request) needs its own merge-queue script as a `batch` command step. The `prs` step opens the PR, and harvest only marks it `harvest: pr`. Nothing in ASF merges a PR once its checks are green. So every PR-landing product must carry landing machinery of its own, which is exactly the legacy the first customer is removing (operator 2026-09-24: "only ASF from now").

Want: a native landing for the PR lane.
- The `prs` step (or harvest) merges an ASF-opened PR once it is approved per the approvals matrix (merge_routine_pr / merge_amendable_set) and its required checks are green.
- It uses the repo's merge method: the GitHub merge queue if one is enabled, else squash or rebase as configured.
- It honors the same S1-first order, `after:` holds and one-hotfix-first rule as the wave.
- Red checks send the branch back to its session, the same way as a red local gate.
- `steps.batch` becomes optional for PR products. A product script there still runs if set.
- Capacity: at most capacity.batch.parallel merges in flight, per_run per tick.

Tests use a fake gh:
- green + approved → merged
- red → held and sent back
- pending → waits
- human-now approval → NEEDS OPERATOR line, not merged
- merge-queue repo → enqueued, not merged directly

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
