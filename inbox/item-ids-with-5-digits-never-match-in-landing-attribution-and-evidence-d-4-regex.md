# Item ids with 5+ digits never match in landing attribution and evidence (\d{4} regex)

The item-id token regex `\b[EFSTBDR]-\d{4}\b` cannot match ids with five or more digits. It appears in asf/record/match.py:28 and in asf/evidence/evidence.py:919 and :921 (ID_TOKEN, BRANCH_ID_TOKEN); asf/record/idcheck.py:20 already uses `\d{4,}`. A landing on `cloud/T-32850` is logged with `item: null` ("no rule matched"), so the landing is unattributed and the Task closes only through ingest's slower landed-green rule. Seen 2026-10-06: a botseon Task stayed New for about 30 min after its PR merged, and its Feature waited with it.

## Acceptance
- One shared item-id pattern accepts 4 or more digits (`T-1234`, `T-32850`) and rejects `T-123`. match.py, evidence.py and idcheck.py all use it, tested on a shared fixture list.
- `match_event` on branch `cloud/T-32850` returns `['T-32850']`; `worker/T-1234` still returns `['T-1234']`.
- A landed branch for a Task with a 5-digit id writes a landings row with a non-null item and closes the Task on that same step (test).
- When a Task reaches a terminal state, any pending `reshape:` note on its card is dropped with a History line (test).
