# CI artifact storage quota: daily prune + quota monitor (recurring red jobs on botseon)

Recurring on botseon, 2026-09-26: CI jobs fail with "Artifact storage quota has been hit". #846's p1-e2e-b failed at 14:47, after a manual prune of artifacts older than 2 days at 11:15. Each hit turns required jobs red and blocks merges and prod.

Expected, as generic ASF behaviour:
1. **Daily prune:** a daily-clock step deletes Actions artifacts older than `ci.artifacts.retention_days` (default 2) through `gh api repos/<r>/actions/artifacts`. It never touches artifacts of the newest run on the trunk or of open PRs' latest runs.
2. **Quota monitor:** each tick reads the used storage (billing/shared-storage, or the sum of artifact sizes). Above `ci.artifacts.prune_at_pct` (default 80%) it prunes early, oldest first. The status Runners row shows "artifacts X GB / Y GB".
3. **Doctor:** a doctor row flags upload-artifact steps without `retention-days`, advisory only; that part is the product's ci.yml.

Test: fake artifact list over the threshold → the oldest deletable artifacts are removed, and protected ones are kept.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
