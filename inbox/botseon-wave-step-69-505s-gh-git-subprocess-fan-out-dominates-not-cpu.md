# botseon wave step (69-505s): gh/git subprocess fan-out dominates, not CPU

botseon's tick wave step runs 69-505s (asf's wave: 6s). Profiled the read-only equivalent
(`python -m cProfile -m asf.cli harvest --dry-run --product botseon`, dry_run guards every
mutation in asf/harvest/lane.py) — no tick or session was run. Wall time 36.8s, of which 34.5s
(94%) sat inside `subprocess.run`/`select.poll` — this step is I/O-bound on `git`/`gh`, not CPU
(only 11.7s of user+sys CPU logged by `time`).

Top 3 costs, by cProfile cumulative time:

1. **`gh` (GitHub API) subprocess calls dominate.** 17 `gh` invocations cost ~20.8s of the 36.8s
   wall (57%), ~1.0-1.5s each (network round trip):
   - `asf/harvest/lane.py:3106 pr_checks()` → `asf/harvest/harvest.py:837 _gh()`, 8 calls,
     8.17s cumtime — one `gh pr checks` per gated PR.
   - `asf/ci_queue.py:506 _gh()`, 6 calls, 8.85s cumtime — from `_list_runs` (ci_queue.py:1246,
     3.04s alone), `cancel_superseded` (ci_queue.py:1193, 1.92s), and `admit`'s
     `run_ids`/`attempt_jobs`/`run_files` (ci_queue.py:517/536/548).
   - `asf/harvest/harvest.py:844 gh_json()`, 3 calls, 3.82s cumtime.

2. **Local `git` fan-out, one process per lane branch per fact.** `asf/harvest/harvest.py:81
   sh()` was invoked 549 times, 11.2s cumtime (~20ms/call), from per-branch functions in
   `asf/harvest/lane.py` during `gather()` (lane.py:1042, 9.4s cumtime overall): `trunk_history`
   (lane.py:513, 116 calls/1.87s), `touched_files` (lane.py:296, 105 calls/1.22s),
   `already_on_trunk` (lane.py:655, 105 calls/1.19s), `branch_facts` (lane.py:1068,
   108 calls/1.16s) — 3-4 separate `git` spawns per branch, times ~30+ branches in the lane.

3. **CI-queue admission re-scans the same items repeatedly.** `ci_queue.relieve_trunk`
   (ci_queue.py:1362) alone costs 9.59s cumtime; the dry-run trace shows the same items
   re-evaluated at growing queue positions in one pass (e.g. T-0141/T-0095/T-0359 each logged
   "would wait" at 14th, 18th, 24th, 25th in line) — the admission scan re-derives the same
   wait reason for the same item multiple times per pass instead of once.

This is one `harvest --dry-run` pass (part of the wave step's `lane_pass`/`gate_pass`); a real
tick wave also runs the feeder's gated invariant replans, brief-building `git` calls, and the
launches themselves, plus retried reword/pushes seen in the tick log — consistent with the
69-505s range observed. No code changed; no gbrain write calls made.
