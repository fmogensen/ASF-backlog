# Stacked builds: dependent Tasks start on the predecessor's branch
parent: E-0003

Stacked builds: a dependent Task starts on its predecessor's branch instead of waiting for its merge (operator-approved 2026-10-10, ASF 0.3).

## Problem
The largest active wait is `after` (waiting for a predecessor Task to land): 332 item-hours, p50 3.4 h, p90 12 h (`asf kernel waits`, 2026-10-10). Plans chain Tasks with `after:`; each link costs a full build + review + CI + merge before the next starts.

## Change
- When a Ready Task's only unmet `after:` predecessors are in Review or Landing with a green head, the kernel launches it on a branch cut from the predecessor's head (stacked). At most `kernel.stack.depth` (seed 1) levels.
- Its PR targets main and is held from merge until the predecessor lands; when the predecessor lands (squash), the kernel rebases the stacked branch onto main (drop the predecessor's commits) and re-runs CI; a rebase conflict becomes a fix round with the conflict as its finding.
- If the predecessor gets changes requested, stacked children are rebased onto its new head after its fix round; if it is retired, children are rebuilt from main.
- `after:` edges that a plan wrote only because of shared files are not treated as code dependencies when the kernel's own file-overlap serialization already covers them (flag on the plan Task: `after_reason: files|code`; missing = code).
- Out of scope here (0.4): removing hotspot files (self-registering commands instead of edits to asf/cli.py, count asserts in tests/test_surface.py).

## Acceptance
- A Task whose predecessor is in Review with a green head launches stacked on the predecessor's head (test).
- The stacked PR cannot merge before the predecessor lands (test).
- After the predecessor squash-lands, the stacked branch is rebased onto main with only its own commits and CI re-runs (test); a conflict yields a fix round naming the conflict (test).
- A predecessor fix round or retirement re-stacks or rebuilds the child (test).
- On the live record after install, `after` wait p50 drops; measured and reported against the 2026-10-10 baseline (3.4 h p50, 12 h p90).
