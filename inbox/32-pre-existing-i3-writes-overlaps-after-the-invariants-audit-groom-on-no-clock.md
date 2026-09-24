# 32 pre-existing I3 writes: overlaps after the invariants audit; groom on no clock

After the fix-package install (36f6917), `asf check --invariants --product asf` reports 32 I3 violations: Active tasks whose `writes:` intersect (e.g. T-0056 vs T-0183 on tasks/T-0183.md). They predate the package; the audit is new. Want: the record resolves them by policy (the lane/feeder already refuses a new overlap), not by hand edits: either the overlapping Active tasks are serialized with `after:`, or I3 ignores a task's own card file in `writes:`. Also: doctor shows NEEDS OPERATOR "step groom is on no clock".

## Question
Which Epic is this under? No open Epic shares a title word with it.
