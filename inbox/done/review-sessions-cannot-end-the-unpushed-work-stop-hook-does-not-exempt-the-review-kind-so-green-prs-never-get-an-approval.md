→ B-0276

# S1: review sessions cannot end — the unpushed-work Stop hook does not exempt the review kind, so green PRs never get an approval
signature: Stop hook asf hook unpushed refuses to end a review session (NO_LANDING_KINDS lacks review)
severity: S1

The unpushed-work Stop hook (`asf hook unpushed`) exempts only `NO_LANDING_KINDS = ('groom', 'groom-clerk')` (asf/workers/lifecycle.py:2514). It therefore refuses to end a `review` session whose protocol leaves `docs/reviews/N-<id>.md` deliberately uncommitted. The review loops or stalls, reaches the daily relaunch cap, and is parked. No approval is ever written, so the factory never requests a land, and PRs that are green and CLEAN on their exact head wait for hours.

Seen 2026-10-06 on ASF: #842, #848, #849, #858 and #859 had green checks for 1–5 h with no approval. The tick log has INPUT rows from review-t-0589, review-t-0218 and others naming this hook. A heartbeat loop that the sandbox refuses adds "stalled: no beat" failures (review-t-0755).

This is a deadlock for any product under `factory_only`: the fix's own PR needs a review that this defect blocks.

## Acceptance
- A run of kind `review` (and any run kind whose protocol forbids committing) is exempt from the unpushed-work Stop hook. A test shows a review session with an untracked review file ending cleanly.
- A review session that ends with its review file written is recorded as finished, and the PR gets an approve or changes verdict for its current head.
- A refused heartbeat loop does not mark a session stalled while it is still producing events. Tested.
- A review whose worktree is gone is recreated or relaunched once, not left DEAD.
- A PR with green required checks on its head and no review verdict for that head for more than N minutes (configurable) raises one watchdog breach naming the blocking stage, and shows as a row in `asf status` and `asf doctor`.
