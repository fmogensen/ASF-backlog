# asf retire refuses a Task under a retired Feature: it flags itself as a removed reference
parent: F-0346

Type: Bug

`asf retire <Task>` is refused when the Task's parent Feature is already retired. The record pre-commit reports "tasks/T-0377.md:11: blocking: delivers references removed item T-0377": the Task being retired is flagged as a removed reference to itself. Reported by the botseon console on 2026-10-12 for T-0377 through T-0381 (parent F-0112 retired).

Acceptance:
- Retiring an item never fails the removed-reference check on that item's own id.
- Retiring a Task whose parent is retired succeeds.
- Test: a record with a retired Feature and a live child Task; `asf retire` on the Task commits clean.
