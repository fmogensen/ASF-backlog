# No command releases a stale correction; F-1129 holds one for a never-pushed branch

A killed first-death session (botseon spec-f-1129, 2026-09-27, killed because blockedBy was not yet honoured) left a "died twice" correction in the ledger; 2fabf6c stops new ones, but the recorded one stays and would launch a correct session on a never-pushed branch when B-1382 closes. There is no command to release a correction; add one (asf correction drop <item>) or have the lane drop corrections whose branch never reached origin.

## Question
Which Epic is this under? No open Epic shares a title word with it.
