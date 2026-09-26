# Lane leaves a CONFLICTING PR in MERGING→WAITING with a misleading --auto refusal

botseon #818 (cloud/direct-F-0113) has been mergeable=CONFLICTING since 02:42Z; each tick the lane goes MERGING→WAITING and the refusal alternates between "merge conflicts" and gh's misleading "add the --auto flag".

Expected: merge() reads `mergeable` first; CONFLICTING routes to the factory rebase / a correct session instead of WAITING, and the held line names the conflict.

Test: fake gh returning CONFLICTING → lane dispatches a rebase, not WAITING.

## Question
Which Epic is this under? No open Epic shares a title word with it.
