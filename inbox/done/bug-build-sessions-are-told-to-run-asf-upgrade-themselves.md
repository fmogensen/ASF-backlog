→ B-151002

# Bug: build sessions are told to run asf upgrade themselves
parent: F-0346

type: bug
parent: F-0346

Bug: build sessions are told to run `asf upgrade --product asf` themselves. A NEEDS OPERATOR hint in the code tells a session whose pinned CLI lacks a subcommand (e.g. `pre-push`) to upgrade the product pin. On 2026-10-10 two build sessions (build-t-129157, build-t-129191) followed it from their worker HOME: they rewrote the real pin, loaded the paused 0.1 floor clocks into the real launchd, and retired the kernel jobs, taking the kernel down for 9 min and then 8.5 h. PR #1451 now refuses launchd changes from a foreign HOME, but the hint still sends sessions to an operator-only command.

## Acceptance
- No brief, report hint or NEEDS OPERATOR text tells a session to run `asf upgrade`, `asf kernel install` or `asf scheduler` (test over the templates and hint strings).
- A session whose CLI lacks a subcommand reports it as a finding, and the kernel moves the pin itself when the configured upgrade policy allows (test).
