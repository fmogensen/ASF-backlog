→ closed (groom 2026-10-06, adjudicator, groom-2026-10-05)

# Plan-Task heading regex misses '### Task T1:' so whole plans never mint

Seen 2026-10-04 on botseon F-0001: docs/superpowers/plans/f-0001.md uses headings "### Task T1: ..." and its 8 Tasks were never auto-minted, because the plan-Task heading regex does not match that form. Operator minted T-0584..T-0591 by hand via inbox+groom. Fix: accept "### Task T<n>:" (and similar) headings, and report a plan whose Task headings match nothing instead of silently minting zero. Also: asf new cannot set stories: or create Features, so minting needs inbox+groom.

## Question
Which Epic is this under? No open Epic shares a title word with it.
