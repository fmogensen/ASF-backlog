→ F-0258

# Reserved identifiers (migration numbers, bands) are checked against every open PR head, and a brief-named reservation is honoured

Parent: E-0001

Two open PRs can each claim the same numbered resource, such as a migration number or a register band. Each passes its own pre-push check against the trunk, and the clash only shows when both meet in a batch. Seen 2026-10-06 on botseon: a Task's brief named migration 0320, but the coder booked 0319, which an open replan PR already held. The pre-push check gives the product no list of open PR heads, and nothing makes the coder keep a reserved identifier the brief names.

## Acceptance
- ASF exports the open PR heads to the product's `pre_push_check` (and `gate_checks`) as `ASF_OPEN_PR_HEADS` (sha and branch per line, the session's own PR excluded). A product's reservation script can then check against the trunk plus every open head. Tested with a fake host.
- A product may declare `conventions.reservations: [{pattern: <regex>, files: <glob>}]`. ASF checks that no two open PRs, or a PR and the trunk, add the same matched identifier, at pre-push and before a batch cut. A clash names both PRs and the identifier, and blocks only the newer PR. Tested.
- An identifier the brief names under `reserved:` (or that matches a declared reservation pattern in the brief) is checked at pre-push. A head that uses a different identifier for the same pattern is refused, with the brief's value in the message. Tested.
- All of this is generic config; no product names appear in code.
