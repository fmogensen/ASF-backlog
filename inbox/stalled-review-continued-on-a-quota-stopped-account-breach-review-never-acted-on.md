# Stalled review continued on a quota-stopped account; BREACH review never acted on
parent: F-0346

Type: Bug

A review whose cloud session stalls is continued in the same session, on the same account, even when the quota guard has that account at stop or cooldown. The review then sits idle until the next 45-minute stall verdict, and its PR cannot land meanwhile.

Observed 2026-10-12:
- review-t-154345 stalled after "no beat 45m" at 06:57Z and was continued on its account, which was at 98% weekly quota (stop).
- review-t-159849 stalled the same way at 06:57Z and was continued on an account at 93% (cooldown).
- The kernel logged "BREACH ... review 1.2h -> none" on every tick and took no action. PR #1611 (CLEAN, MERGEABLE) and PR #1640 waited behind them, and nothing merged from 04:50Z to past 07:15Z.

Acceptance:
- Before a stalled session is continued, the kernel checks its account against the quota guard. If the account is at stop or cooldown, the work is relaunched fresh on a free account instead of being continued.
- A review that is in BREACH for longer than a configurable limit (kernel yaml attribute) is ended and relaunched, rather than logging "-> none" forever.
- Tests: a replay with a stalled review on a stopped account relaunches it on a free account; a BREACH past the limit produces a relaunch action.
