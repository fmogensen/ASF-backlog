# Cross-product seat overcommit: two waves launch on one account at once (6 of cap 4)

Proven 2026-09-26 13:28:57–13:29:04Z: the botseon and asf waves both launched on one account within 7 s. Each wave reads the seat ledger once at its start, so that account reached 6 sessions against a cap of 4.

Expected: seat claims are atomic across products, using a lock or compare-and-set on the ledger per account at `take()`. A wave re-reads the account load under the lock just before spawning.

Test: two concurrent waves never exceed an account cap.
