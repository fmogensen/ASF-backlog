→ S-175311

# The kernel installs its own merged fixes (auto-upgrade behind the shadow diff, with rollback)
parent: F-0356

type: story
parent: F-0356

The kernel installs its own merged fixes. Measured 2026-10-11: the S1 seat fix (B-160759, PR #1548) merged on green but stayed uninstalled for hours, so seats sat at 6/18 with 44 Ready and no merges for 34 min until the console installed main's head (0.2.117) by hand. Kernel fixes reach production only when a person installs them.

## Acceptance
- With `kernel.upgrade: auto` (product yaml), the watch notices a main head carrying kernel changes, required checks green on that head, newer than the running venv. It installs that head and runs the shadow diff, and swaps only when the diff is within `kernel.upgrade.max_state_changes`; otherwise it reports the diff as a Stuck(operator) row (test).
- The install moves the product pin with it, keeps the previous venv for rollback, and rolls back by itself if the first tick after the swap fails or no tick runs within 2 intervals (test).
- `kernel.upgrade: pinned` keeps today's behaviour (test).
