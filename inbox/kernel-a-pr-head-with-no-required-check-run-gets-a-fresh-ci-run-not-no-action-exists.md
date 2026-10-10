# Kernel: a PR head with no required-check run gets a fresh CI run, not 'no action exists'

parent: F-0338. Kernel: an open kernel PR whose head has no required-check run at all (last run cancelled, or never started) gets "no action exists" forever. Live: T-0763 / PR #1084 sat 1.2 h in Review with no checks after its hung run was cancelled; fixed by hand at 16:36Z (close+reopen re-triggers the pull_request run).
Fix: such a PR gets a fresh CI run from the kernel (close/reopen or workflow dispatch), once per head, logged.
Acceptance: a test where a PR head has no required checks leads to a CI re-trigger action, not a breach with no action.
