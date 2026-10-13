→ B-211642

# A review finding's writes request reads cited words as paths, and a granted route loses the finding
parent: F-0358

Type: Bug

The review finding's writes request (T-206244, PR #1719) ships off (kernel.resolve.finding_writes: false). Two defects must be closed before a product turns it on.

1. The path reader takes cited words for paths to change. Measured over 283 changes verdicts on cards with a footprint: 191 (67%) read as a request and 31 (11%) would have gone to the operator. Words it read as paths: a caller the finding cites (asf/kernel/model.py), bare module names (decide.py, ports.py), origin/main, try/except, .md, a branch name. Granted, these widen `writes:` with words that are not paths and serialise the item behind every holder of a hub file; under kernel.risk.high they park the item. asf/kernel/findings.py `outside_writes` must tell a path the finding asks to change from one it only cites, and accept only paths that exist in the repo or that the finding says to create.

2. A routed finding is ruled whatever the operator answers. asf/kernel/apply.py `FindingWrites` marks the finding's key ruled when it routes to the operator, so when the operator grants the paths the finding is never briefed again and the work is lost. A grant must widen the footprint and carry the finding into the next round.

## Acceptance
- [ ] On a fixture of real review findings holding each word shape above, none is read as a writes request; a finding that names a file to change outside the footprint is.
- [ ] A routed finding the operator grants is briefed in the next round with the widened footprint; one the operator refuses is ruled.
- [ ] With the key on, replaying the measured verdicts requests writes on under 10% and routes under 2% to the operator.
