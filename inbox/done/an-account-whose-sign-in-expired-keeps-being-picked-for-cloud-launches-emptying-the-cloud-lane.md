→ B-224009

# An account whose sign-in expired keeps being picked for cloud launches, emptying the cloud lane
parent: F-0360

Type: Bug

An account whose sign-in has expired keeps being chosen for cloud launches, so the whole cloud lane sits empty. Seen 2026-10-14 06:55: one worker account came back from its quota stop at 06:00 with 0% used, so the cloud lane picked it first; every cloud create failed with "helper create: no RemoteTrigger call made (exit 1; Failed to authenticate: OAuth session expired and could not be refreshed)", each launch fell back to a local seat, and ASF ran 1 of 10 seats with work Ready while two other accounts were signed in and free. `asf cloud doctor` reported the lane ready.

## Acceptance
- [ ] A cloud create that fails with an authentication error stops that account for the cloud lane (as a quota stop does) and the same tick's launch goes to the next free account.
- [ ] The stop shows in `asf workers quota` and in the IDLE line with the reason "sign-in expired", and is lifted when a launch on that account succeeds or the operator re-signs in.
- [ ] `asf cloud doctor` checks each cloud account's helper login and reports an expired one as not ready.
- [ ] The kernel raises one operator notice per expired account, not one per tick.
