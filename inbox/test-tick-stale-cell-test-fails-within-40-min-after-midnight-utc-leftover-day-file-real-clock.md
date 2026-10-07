# test_tick stale-cell test fails within 40 min after midnight UTC (leftover day file; real clock)

Parent: E-0001
severity: S2

tests/test_tick.py:500-503 (the stale-cell test) fails whenever it runs within 40 min after midnight UTC. `at(20)` writes a tick line into today's metrics file. `at(40)` then removes and rewrites only yesterday's file, so today's fresher line from `at(20)` survives, `stale_cell` sees a tick 20 min old, and it returns None. Seen 2026-10-07 00:24Z: main went red on the B-0273 merge (v0.1.209, run 37550943914, "AssertionError: None != 'STALE since 23:41 …'"). The product code is right; the test is not hermetic in time. Red main runs count against release-readiness criterion 3 and PR first-pass health.

## Acceptance
- The test pins "now" through an injectable clock or a fixed time, and clears every metrics/ticks file between cases. It passes for a fixed now of 00:10Z, 00:39Z and 12:00Z (parametrised).
- A repo-wide check finds no other test that builds a day file name from the real clock, or is fixed by the same seam.
