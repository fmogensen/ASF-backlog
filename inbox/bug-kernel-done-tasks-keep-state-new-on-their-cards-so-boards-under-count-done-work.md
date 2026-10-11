# Bug: kernel-done Tasks keep state New on their cards, so boards under-count done work
parent: F-0346

type: bug
parent: F-0346

Bug: a Task the kernel judges done keeps `state: New` on its card. Measured 2026-10-11: of the 0.3 Tasks, 19 carry `kernel_state: done` (18 of them have a commit on origin/main naming them), yet every one still reads `state: New`. Anything that reads `state:` (the backlog board, release readiness, the console's own count) reports 0.3 as ~1 of 57 done when it is 19.

## Acceptance
- When the kernel judges a Task or Bug done on main, the same record commit sets its card `state:` to Closed, with the trunk sha in History (test).
- A one-shot repair by code closes existing cards whose kernel_state is done and whose work is verified on origin/main (test on a fixture, then run live).
- `asf backlog` and release readiness count by the same done rule the kernel uses (test).
