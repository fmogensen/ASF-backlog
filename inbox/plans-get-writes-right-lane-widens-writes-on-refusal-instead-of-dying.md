# Plans get writes: right; lane widens writes: on refusal instead of dying

Feature: plans get `writes:` right the first time, and the lane widens `writes:` instead of dying.

Evidence: botseon T-0338 was refused with "file outside writes:" because a Task touched a file that docs quote (the docs-check pattern).

1. The plan brief (asf/briefs/templates/plan*.md) tells the planner to add every doc that quotes a changed file to `writes:`.
2. When a push is refused naming a file outside `writes:`, the lane widens the Task's `writes:` (and checks it against Active overlaps) and resends. It must not mark the Task dead.

Acceptance: a lane test where a refusal naming a file outside `writes:` leads to a widened `writes:` and a resend, and an overlap with an Active Task holds the Task instead.
