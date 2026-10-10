# Idempotent reconcile of ledger-shaped proof claims into the record

Reconcile externally recorded proof claims of shape {story, line, test, pr, sha} into the record, idempotent (tick once, never reverse); prefer the floor's own evidence scan of the merged PRs, the external file as cross-check. Acceptance: running twice ticks each line once; a refused claim never ticks.

parent: F-0334 (ASF 0.3). Generic: no product named in code.
