→ F-0126

# feeder: a Task with no writes: (migrated pre-ASF plan) is launched and can only block

The feeder launches PLAN → CODE for a Task whose card has no `writes:` (a Task migrated from a
pre-ASF plan that lacks the machine-read lines `stories:/writes:/after:`). The coder brief then says
`writes: (none)`, so every file is outside its footprint, and the session can only report
`status: blocked` — a wasted launch. Seen twice in one tick on a migrated Feature whose spec/plan
also still sat on unmerged pre-ASF branches (one Task was an old-process "integrator" task that ran the
product's legacy merge queue).

Expected:
- a Task with no `writes:` is never launched; it shows a non-launching row (e.g. `RESHAPE → PLAN`
  or `NEEDS FOOTPRINT`) so the groom/plan session re-cuts it with the machine lines;
- a Feature whose `links.plan` file is not on the trunk does not feed PLAN → CODE;
- a coder that reports `status: blocked` with NEEDS OPERATOR is not relaunched until the card changes.
Acceptance: feeder test — a migrated Task with no writes: yields no launching row.
