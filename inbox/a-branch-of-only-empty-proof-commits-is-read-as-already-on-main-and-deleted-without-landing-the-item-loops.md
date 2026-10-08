# a branch of only empty proof commits is read as already on main and deleted without landing; the item loops

Severity: S2

Reported by botseon on 2026-10-08. A branch whose commits are all empty (proof-only commits with no tree change) is read as "already on main", because its tree equals the trunk's. It is then deleted without landing. cloud/T-41867 was deleted as "already on main at daeef1c", so its proof commits never reached main and T-41867 looped. The workaround was a ruling that cut the branch fresh with one real commit.

## Acceptance
- A branch with commits not reachable from the trunk is not "already on main" because its tree matches. Commits whose messages carry a proof, a ticked acceptance or an item id the trunk doesn't name land, or are refused by name, and are never deleted silently. A test covers it: a branch of two empty proof commits on the trunk head is not reaped, and draws a landing row.
- A branch whose every commit is a patch-equivalent of a trunk commit (`git cherry` all `-`) is still reaped as today. The existing test stays green.

## Question
Which Epic is this under? No open Epic shares a title word with it.
