→ F-0140

# The factory's own clock imports from the live dev checkout — a tick mid-update gets a torn tree; tick from a snapshot

The factory's own clock runs `python -m asf.cli tick` straight from the development checkout, with PYTHONPATH pointing at the checkout. Harvest fast-forwards that checkout, and the operator's sessions pull and push in it. A tick that imports while the files are changing gets a torn tree. Seen 2026-09-24: health, prs and harvest all failed with "cannot import name 'DEFAULT_WIDEN_MAX_FILES' from 'asf.conventions'" for one tick, while a new module and its dependency landed in two separate file writes. The clock exited 1.

Want: the self-hosting product ticks from an immutable snapshot.
- At tick start, resolve the checkout's HEAD sha and run from a cached worktree or export at that sha (`state/<product>/code/<sha>`, reused while HEAD is unchanged, older ones pruned).
- Or use a pinned install that harvest re-pins after each landing (`asf upgrade --to <sha>`).
- The scheduler renders that command. doctor shows which sha the clock runs.

Tests: a tick started while the checkout moves still imports one consistent tree; the snapshot is reused while HEAD is unchanged.
