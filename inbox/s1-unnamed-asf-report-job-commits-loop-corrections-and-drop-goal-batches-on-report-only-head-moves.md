# S1: unnamed 'asf: report <job>' commits loop corrections and drop goal batches on report-only head moves

Parent: E-0001
severity: S1

The report-commit naming loop drops goal batches. The cloud brief (asf/workers/cloud.py ~429) tells a session to end with a commit whose subject is `asf: report <job>`, which doesn't name the item. The lane's naming check then refuses the branch. When the branch also carries a trunk-copy commit, the lane's own reword bails ("1 commit(s) are copies of origin/main commits — rebase onto origin/main"), writes `kind=naming` and goes BACK. A new correct session launches and pushes another unnamed report, and the head moves. A moved head drops the batch the PR sat in, along with every other member. Seen 2026-10-07 on botseon T-0659 (#1122): correct sessions at 06:14, 06:47, 08:07 and 09:53Z pushed only reports after the code fix had landed on the branch. At 12:07 a report moved #1122's head and dropped batch 09:44Z, taking the green, docs-only #1230 (a goal Feature's plan) with it.

## Acceptance
- Every brief (cloud, local, actions) gives the report subject as `asf(<item>): report <job>`, which names the item, and spawn.REPORT_SUBJECT and evidence.REPORT_SUBJECT accept both forms. Tested.
- The lane's naming check never refuses a report commit (one matching REPORT_SUBJECT, empty or report-only). Tested.
- A head move that adds only report commits (an empty tree diff against the previous head) neither turns the lane BACK nor drops the batch. The batch keeps the PR at its previous head, or carries the green over an identical tree (B-0275 tree equivalence). Tested.
- A `kind=naming` hold whose only cause is trunk copies is cleared by the lane's own rebuild first, with no session (the same line as the dead-review relaunch item). Tested.
- A batch dropped for a member's head move re-cuts its other members in the same pass. Tested: a 2-member batch, one member's report-only move, the other still lands.

## Root of the "copies" bail (botseon, 12:20)
`git cherry` marks the branch's EMPTY report commits as copies of origin/main, because their empty patch matches the empty report commits already on main. So the lane's copies check (asf/harvest/lane.py ~659, ~2642) fires on every branch that carries an empty report commit, and that is what blocks the naming reword. Added acceptance:
- [ ] The copies check ignores empty commits (and REPORT_SUBJECT commits). An empty report commit is never counted as a trunk copy. Tested: a branch with one fix commit and two empty report commits has 0 copies.
