→ F-0153

# Naming reword retries a refused push every tick (144 log lines, 3 branches)

The naming repair retries a refused reword push every tick. botseon's tick log has 144 lines of `reword <branch> (naming) push refused: error: failed to push some refs` across three branches (cloud/spec-tinkerer-mode, cloud/team-staffing-t3, cloud/factory-session-ledger-t1), 48 each, two per tick. "Back to its session" never takes effect.

Expected: after a refused reword push, the repair records the refusal (branch and tip sha) and does not retry until the tip changes. It logs the git stderr reason (for example non-fast-forward or a protected ref) once. If the branch has no live session, it routes to a correct session or to the retention reaper.

Test: fake push refusal → a second pass on the same tip does no push and logs nothing new.
