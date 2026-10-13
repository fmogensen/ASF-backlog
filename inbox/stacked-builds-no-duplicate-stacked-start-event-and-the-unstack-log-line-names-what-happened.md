# Stacked builds: no duplicate stacked-start event, and the unstack log line names what happened
parent: F-0361

Type: Bug

T-160211 (PR #1688, stacked builds: the knobs and records) was landed by the console at a green head after three reviews, to close milestone 0.3. Its third review left two findings in asf/kernel/apply.py, neither changing what the kernel decides:

- :262 — guard the `stacked-start` write with `if stack_of(self.facts.items.get(a.item_id)) != (a.base, a.base_sha)` so a re-planned NoteStack (a NoSeat launch, decide.py:2337-2341) appends no duplicate event.
- :274 — return "PR #N -> trunk" only when the plan carries a Retarget for that PR, else "PR #N"; the teardown path (ArchiveAndReset) currently logs "-> trunk" for a PR it closes.

Acceptance:
- Two identical applies of NoteStack leave the record's events one entry long (test).
- The unstack result line says "-> trunk" only for a retarget (test in tests/kernel/test_stacked_builds.py for the teardown path).
