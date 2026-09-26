# Tick performance round 2: flaky pass window, git status, card parse cache, lane git cache, batched evidence reads

Follow-ups from the 2026-09-26 tick profile (29654dc1c cut the tick's Python CPU from 91 s to 22 s). These are measured, not guesses:

- **Flaky pass** (asf/tick/flaky.py:286): it pages a week of every workflow run on each tick, 26–31 s of wall time, and silently truncates at the 1,000-result cap. Narrow the since-window to the previous pass, or use the workflow's own runs endpoint. Saves about 25 s per tick and fixes the truncation.
- **Worktree `git status`** in health: use --untracked-files=no, or cache per HEAD and index mtime. About 5 s of git CPU per health step.
- **load_items / frontmatter.parse:** parses about 1,300 cards 15 times per record step. Cache per file. About 1–2 s of CPU.
- **Lane git calls** (touched_files, already_on_trunk, own_commits): cache across the lane and gate passes, keyed on the resolved ref shas. About 450 diff, 270 cat-file and 225 rev-list calls per tick remain.
- **evidence.discover and plan_order:** 104 separate read_ref calls. Batch them into one cat-file --batch. About 8 s.

Profiles and harnesses: the ASF controller session's scratchpad, tick-profile artifacts.
