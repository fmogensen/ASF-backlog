# merge queue drops a batch as red when the host cancelled the run and the required jobs are only skipped
type: feature

Severity: S2

Reported by botseon on 2026-10-08. The merge queue dropped batch …163914-e6eb3a8 (PRs #1294, #1295 and #1254) as "red on gate (skipped), gate-tests (skipped), m6-e2e (skipped)…". The same pass logged that the host had cancelled the run: rules hit its 10-minute timeout under runner contention, "not a verdict — re-run requested". PR #1247's red at 14:39 has the same signature. A host-cancelled run whose downstream jobs are skipped is not a verdict on the batch.

## Acceptance
- A batch whose run was cancelled by the host (a timeout or cancel classified "not a verdict"), and whose other required jobs are `skipped`, is requeued with its members intact, not dropped. A test covers the e6eb3a8 shape.
- The merge queue never names a `skipped` job as a red reason. A required job that is skipped because an upstream job was cancelled reads as "no verdict".
- A batch with a real `failure` on a required job is still dropped, and its members are bisected as they are today (the existing test stays green).

## Question
Which Epic is this under? No open Epic shares a title word with it.
