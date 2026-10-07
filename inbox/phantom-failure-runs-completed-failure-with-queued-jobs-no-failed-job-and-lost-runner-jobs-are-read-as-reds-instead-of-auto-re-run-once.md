# Phantom-failure runs (completed/failure with queued jobs, no failed job) and lost-runner jobs are read as reds instead of auto re-run once

Parent: E-0001
severity: S2

Two kinds of infra-red CI runs are read as real reds and wait for a hand re-run.
(1) A phantom failure: a run reports status=completed, conclusion=failure while some of its jobs are still `queued`, some required jobs have no job at all, and no job failed. It is not in ci-cancels.
(2) A lost runner: a job ended failure with no runner_name, no failed step, and logs that return BlobNotFound.
Seen 2026-10-07 on botseon: #1143 run 37641485608 at head 47be7bdd1 (phantom), and #1235 gate job 112862304581 (lost runner). Both held land-requested goal PRs until someone re-ran them by hand. One head moved before the re-run.

## Acceptance
- The CI health pass classifies each red run. A phantom (conclusion failure, no failed job, jobs still queued or missing) and a lost runner (failed job with no runner and no failed step) are both infra, never a test red. Tested on fixture run JSON.
- An infra red on the PR's current head is re-run once automatically (`rerun --failed`, head-matched) and recorded with its class in the cancels/infra ledger. A second infra red on the same head raises one watchdog breach naming the class. Tested.
- Status and the land-request line show "infra red: re-run queued", not a red check. Tested.
