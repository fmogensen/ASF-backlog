→ B-0130

# Invariant EVENT I9 repeats on every tick for one past merge

type: bug
severity: S3

The invariants check repeats one event every tick without end: botseon's tick log prints `EVENT I9: cloud/plan-T-0140 merged outside the lane (no MERGING intent)` on each of dozens of ticks since that plan branch merged natively. The event is about a single past merge; repeating it buries real events in the log and in the digest. Want: an invariant EVENT about a past fact (a merge outside the lane) is emitted once, recorded in the product's state with its key (branch + merge sha), and not re-emitted on later ticks. Tests: two ticks over the same merged branch emit the I9 event once.
