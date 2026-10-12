→ B-184865

# Bug: tests/kernel/test_ports.py hangs locally on real git
parent: F-0346

type: bug
parent: F-0346

Bug: tests/kernel/test_ports.py hangs locally on real git (observed 2026-10-12 by a fix agent; CI passes). Follow-up to B-157108 (test_host/test_clean_floor under the launchd guard): local suite runs by sessions and agents stall or skip this module, so their local verification is incomplete.

## Acceptance
- test_ports runs against fixture repos only (file:// remotes, no network, no real origin), with a per-test timeout, and finishes locally in under 60 s (test run).
- The suite's git wrapper refuses any non-file remote fetch or push from test_ports, as it does for pushes (test).
