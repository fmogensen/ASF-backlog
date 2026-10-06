# Merge queue: drop a batch as soon as a required job is red, without waiting for non-required jobs

A batch whose required check concluded red stays pending while its workflow run is still live, because the triage waits to re-run it (merge_queue.py: "A red the triage will re-run keeps the chain"). A non-required job, such as botseon's `site`, keeps the run live for a long time. The batch and every batch stacked above it then hold the queue with no chance of landing. Seen 2026-10-06 on botseon batch 1ce016d (#1122, #1194, #1195): gate-tests was red and `site` was still in progress.

## Acceptance
- When a required check concludes red and every required check on that sha has concluded, the batch is decided at once. It is dropped, or it is re-run under the triage's rule. Non-required jobs still running on that sha do not hold it.
- A re-run under the triage's rule cancels the live run first, so the host accepts the re-run. It does not wait for the run to end.
- On a drop, the batches stacked above it are cut again onto the trunk in the same pass, and the dropped batch's live runs are cancelled (MQ_DROPPED).
- Tests: required red + non-required in_progress → drop (or cancel + re-run) in one pass; stacked batches recut; all-required-green + non-required running → unchanged behaviour.
