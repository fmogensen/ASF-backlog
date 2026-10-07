# Session leftovers are wip-committed onto batched/land-requested PRs, dropping goal batches; stranded staged record writes block the sync

Parent: E-0001
severity: S2

When a session ends with uncommitted files (a review's .sdd-input/reviews/*.md, report leftovers), the factory's leftover handling (B-0094) commits them to the PR branch. That moves the head, and a batched or land-requested PR loses its batch. Seen 3 times on 2026-10-07 on botseon goal PRs: #1070 at 21:30Z (wip commit 537670860), #1238 and #1143. Separately, the operator record checkout carried staged tick writes from about 21:16 that blocked the new sync ("working tree has local changes", named once) for 3 h until stashed by hand.

## Acceptance
- When the branch's PR is batched, land-requested, or green on its head, session leftovers are saved to `refs/asf/leftovers/<job>/<ts>` (or the record), never committed to the PR branch. The head doesn't move. Tested.
- A record checkout with staged or uncommitted writes and no live writer for more than N minutes (configurable) has them committed as "record: stranded writes" by the sync when they are card or record paths, or stashed with a named stash when they aren't, so the sync never stays blocked. Tested.
