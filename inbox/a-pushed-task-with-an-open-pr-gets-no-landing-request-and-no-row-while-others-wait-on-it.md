# a pushed Task with an open PR gets no landing request and no row while others wait on it

Severity: S1

Reported by botseon on 2026-10-08. T-47333 (F-0119's last Task) had its PR #1281 open. `next` showed "PUSHED → LAND — pushed work, no other row" with no landing request, while 3 rows waited on it. Botseon landed it by hand with `land --priority`.

## Acceptance
- A pushed Task with an open PR always has a landing-queue entry. If none exists, the tick requests one, and the row names the queue state. A test covers it: a PR open, no landing request → one request after a tick.
- A "pushed work, no other row" Task with waiters is listed among the top blockers (see the blocker-weight S1).

## Question
Which Epic is this under? No open Epic shares a title word with it.
