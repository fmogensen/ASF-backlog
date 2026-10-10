→ B-126941

# Kernel: a PR head with no required-check run gets a fresh CI run, not 'no action exists'
signature: Kernel: a PR head with no required-check run gets a fresh CI run, not 'no action exists'
parent: E-0003
severity: S2

Fix: such a PR gets a fresh CI run from the kernel (close/reopen or workflow dispatch), once per head, logged.
Acceptance: a test where a PR head has no required checks leads to a CI re-trigger action, not a breach with no action.
