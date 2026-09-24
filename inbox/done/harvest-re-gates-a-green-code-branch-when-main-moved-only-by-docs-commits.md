→ B-0110

# Harvest re-gates a green code branch when main moved only by docs commits
signature: harvest: held main moved again on retry, after a green gate, when the new trunk commits touch no file the branch wrote
parent: E-0002
severity: S2

A code branch that passed the gate is held on "main moved again on retry". That happens even when main moved only by docs, spec or plan commits that touch none of the branch's files.

Seen 2026-09-24 01:27: worker/T-0068 and worker/T-0086 were green after a full gate (604s, 60 modules), then both were held. A 10-minute gate loses to any landing in between, so code branches can livelock while docs keep landing.

Fix: after a green gate, if main has moved, check the paths of the new commits. When they are non-code (docs/, specs, plans, backlog) and don't intersect the branch's writes, rebase and land without re-gating. Re-gate only when the moved paths include code or tests.

Test: the gate goes green, a docs-only commit lands on main, and the branch lands in the same harvest. Then the same with a code commit: the branch is re-gated.
