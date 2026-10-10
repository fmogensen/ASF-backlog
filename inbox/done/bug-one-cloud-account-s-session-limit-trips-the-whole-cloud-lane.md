→ B-138544

# Bug: one cloud account's session limit trips the whole cloud lane
signature: Bug: one cloud account's session limit trips the whole cloud lane
parent: E-0001
severity: S1

Bug (defect): one cloud account's session limit trips the whole cloud lane

Type: bug. Botseon's swarm hit the same account-identity/limit class of defect and fixed it in its own scripts/swarm (commits a5ea65c34, d55f8f05a, e91fa9672 for reference). Observed in ASF today: the cloud breaker tripped on "You've hit your session limit · resets 9:20pm" from a single lane account, and every cloud seat went away for the cool-down.

## Problem (evidence, origin/main)

- asf/kernel/ports.py:1847-1864 `KernelPorts._account(lane_settings)` picks the lane account with the most free seats by cap minus live runs only. It never reads the session-limit stops (`asf/workers/headroom.py` `active_limits`, quota-limits.json) nor `account_auth.blocked()`, which the old floor's pool does read (asf/workers/pool.py:412, :431-440). A limited account has its sessions dead, so it has the MOST free seats and is picked first, again and again.
- asf/workers/remote.py:497-499 / :503-509: a failed routine create only runs `_auth_note`; a session/usage-limit refusal from the helper never calls `headroom.record_limit` for that account (record_limit is only reached from `note_exhausted`, headroom.py:220-227, i.e. from a finished run's result, never from a failed create).
- asf/kernel/ports.py:1973 every failed create calls `cloud.Breaker(self.product, s).fail(...)`; `Breaker` (asf/workers/cloud.py:972-1029) is one per product, keyed on nothing account-specific: `fallback_failures` (3) failures in 30 min trip a 30-min cool-down for the entire lane.
- Once tripped, asf/kernel/ports.py:1879 `capacity()` drops cloud seats to the live runs and :1933 `lane()` sends every launch local — the healthy lane accounts are idle too.
- asf/workers/cloud.py:948-953 `FAILURE_CLASSES`: "session limit" / "hit your limit" match no 'quota' word, so the trip line names the class `create`, hiding the cause.

## Failure scenario

Lane accounts A, B, C, D. A hits its 5h session limit. Next tick `_account` picks A (most free seats); the helper's create fails "You've hit your session limit · resets 9:20pm"; the breaker counts 1; the launch falls back local. Next two launches pick A again (nothing recorded A as stopped) → 3 failures → breaker trips → cloud lane off for 30 min while B, C, D have quota. Repeats after every cool-down until A's reset.

## Invariant / fix

- A per-account limit is that account's state, never the lane's: a create refused with a limit text (`headroom.LIMIT_RE`) records `record_limit(account, reset_at(text) or now+DEFAULT_HOLD)` and does NOT count on the lane breaker; an auth refusal likewise stays per-account (account_auth).
- `KernelPorts._account` (both lanes) skips an account stopped by an active limit or blocked by account_auth, the same rule the pool's pick uses — one shared helper, not a copy.
- `capacity()` counts free seats only on lane accounts that are not stopped/blocked.
- The breaker still trips on lane-wide failures (transport, runtime refusals across accounts); `failure_class` maps limit texts to `quota`.

## Acceptance (tests)

- tests/kernel/test_ports.py: a lane account with an active session limit is never picked by `_account(lane_settings)`; another lane account with fewer free seats is picked instead; with every lane account stopped, `lane()`/`launch` falls back local with a reason naming the limit, not the breaker.
- tests/kernel/test_ports.py: a create that fails with "You've hit your session limit · resets 9:20pm" records a limit for that account until the named reset and leaves `Breaker.data['fails']` unchanged; three such failures do not trip the breaker.
- tests/kernel/test_ports.py: `capacity()` excludes the free seats of a limited or auth-blocked lane account.
- tests/test_cloud.py: `failure_class("You've hit your session limit · resets 9:20pm") == 'quota'`; three transport failures across different accounts still trip the breaker (lane-wide behaviour kept).
- tests/test_remote.py: a helper limit refusal on create calls record_limit for the job's account.
