# Full test suites run locally in parallel and exhaust the host (load 99, swap 95%)

signature: "worker sessions and the landing lane run full test suites on the shared host at the same time"
severity: S2

## Description
- On one 16 GB host, two full test suites ran at once. The first was a worker session (Opus, correcting a Bug) running the product's whole gate including tests, `pnpm gate` → turbo → vitest: 81 node processes, 36 minutes and counting. The second was the landing lane's merge-queue test run in a temp clone.
- Load average reached 99. Swap hit 12.6 GB of 13.3 GB, and macOS filed memory-pressure reports. Another minute of this risks the host (and every other session on it) freezing.
- Nothing in the brief stops a worker from running the full suite locally. The product's own CI on the runners is the intended place for it.

## Expected
- Worker briefs say: run only the fast checks (lint/typecheck/the touched package's tests) locally, and leave the full suite to CI. Or enforce it: a host-level budget, at most one full local suite at a time.
- The tick's health step reports host pressure (load, swap), and the wave holds launches while it is high.
