→ B-195926

# Merge-queue reaper cancels foreign CI runs that share its ref prefix
parent: F-0346

Type: Bug

The merge-queue reaper cancels CI runs on any ref that starts with merge_queue.ref_prefix and is not a live batch. A product whose own tooling uses refs with the same prefix gets its CI cancelled ("mq-reaped ... no batch in flight"). There is no exclude or disable setting. Reported by the botseon console on 2026-10-12.

Acceptance:
- The reaper cancels only runs on refs the queue itself created (recorded in its ledger), never by prefix alone; or a product yaml key lists ref patterns it must skip.
- Test: a run on a foreign ref sharing the prefix is not cancelled.
