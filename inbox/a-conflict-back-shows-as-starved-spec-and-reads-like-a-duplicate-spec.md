# A conflict BACK shows as STARVED -> SPEC and reads like a duplicate spec

A spec PR sent BACK for merge conflicts (95cd7c0/a31570b) shows in asf next as "STARVED → SPEC … would launch spec on <existing branch>". The launched session correctly reuses the branch rebased onto origin/main (brief: APPROVED → LAND), but the row label reads like a fresh spec and alarms product sessions (botseon asked to stop it as a duplicate on F-0092/#842). Label it e.g. "BACK → REBASE <branch> (conflict)" and put the conflict/rebase reason in the brief's Why line.

## Question
Which Epic is this under? No open Epic shares a title word with it.
