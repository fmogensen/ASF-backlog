# CI queue relief re-run of a cancelled PR run holds the queue head for hours, mislabelled as a batch

Parent: E-0001
severity: S2

The CI queue's "relief" entry holds the queue head with a dead re-run. A PR run was cancelled as relief for a higher-priority run, then queued to re-run. The re-run was refused 3 times while the host was offline, and the entry was never dropped. It stayed head of the queue ("rerun:<branch>") for over 3 h, mislabelled `kind: batch` / `item: batch`, so status showed "CI START STUCK batch run … for 204 min" while the merge queue had no batches. Seen 2026-10-07 on botseon (run 37532396976 on cloud/T-0484; ci-queue.json `relief`).

## Acceptance
- A relief entry carries the cancelled run's own kind and item (the PR item, not `batch`).
- A relief re-run whose branch or PR has a newer head, is closed, or is already queued or running under another run, is dropped, not re-run.
- A refusal while the host is offline does not count toward the refusal cap. After the cap, the entry is dropped with one finding naming the run, and never holds the queue head or a CI slot.
- `ci N runs in flight` counts only runs the host reports queued or in progress.
- Tests: a mislabelled relief on a PR run; a relief on a superseded head is dropped; 3 refusals drop the entry and free the head; offline refusals are not counted.
