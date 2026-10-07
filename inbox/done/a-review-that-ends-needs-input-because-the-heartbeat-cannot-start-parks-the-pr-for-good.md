→ F-0279

# A review that ends 'needs input' because the heartbeat cannot start parks the PR for good

Parent: E-0001
severity: S2

A review that ends "needs input" because its heartbeat could not start parks the PR for good. The review brief makes the heartbeat loop a precondition. When the session's sandbox refuses it ("Unauthorized Persistence", or a git-common-dir outside the sandbox), the review ends with no verdict, the relaunch cap counts it as an unchanged cause, and the row is parked with seats free. Seen 2026-10-07 on ASF: B-0351 (#926), T-0312 (#878), T-0745 (#848) and T-0248 were parked, and T-0528 hit its cap of 6 in 24 h. The liveness hotfixes (F-0265) make the runtime the beat, but the local brief still asks for the loop.

## Acceptance
- No brief, local or cloud, makes the heartbeat a precondition. A session whose heartbeat start is refused carries on and files its verdict. Tested by a brief scan plus a run fixture.
- A review run that ends without a verdict because of an environment refusal is not counted against the relaunch cap as a "cause unchanged" launch. Tested.
- A parked review row on a green, CLEAN PR shows in `asf status` after N minutes, with the exact `asf unpark` command. Tested.
