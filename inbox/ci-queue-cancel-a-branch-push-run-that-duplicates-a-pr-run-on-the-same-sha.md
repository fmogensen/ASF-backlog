# CI queue: cancel a branch push run that duplicates a PR run on the same sha

2026-09-26 19:35 botseon: branch worktree-m-p4-t1 had three ci.yml runs on one sha cd5a5e5c3 (two `push` + one `pull_request`), all taking heavy runners while an S1 PR (#858) waited with heavy 12/12. The product's workflow trigger is the primary fix (product-side), but ASF's CI queue should enforce it generically: a non-trunk `push` run whose head sha also has a `pull_request` run (queued or in progress) is a duplicate — the queue cancels the push run (newest duplicate first, never a trunk push, never a deploy), one line per cancel, and never re-runs it. Test: push+PR on one sha → push run cancelled, PR run kept; trunk push never touched.

## Question
Which Epic is this under? No open Epic shares a title word with it.
