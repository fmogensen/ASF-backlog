→ F-0141

# The landing gate refuses a branch that adds a debug-toggle marker
type: feature
parent: E-0002

## What is wanted
The landing gate refuses a branch whose diff against the trunk adds a debug-toggle marker — a
comment a session leaves to disable code while isolating a failure (e.g. `TEMP-DISABLED`,
`DO-NOT-MERGE`, `TODO-REMOVE`). The marker list is product configuration with that default.

## Evidence (reported by the first customer install)
A sub-session's temporary disable comment shipped through two landings before anyone noticed; the
only guard was a remembered instruction to grep the merged head.

## Fix direction
Harvest's gate greps the added lines of the branch diff for the configured markers before running
the test command; a hit sends the branch back to its session with the file and line (same path as a
red gate), never lands it.

## Test
Fixture branch adding a line with a default marker is not landed and its session gets the finding;
the same branch without the marker lands; a marker already on the trunk does not block an unrelated
branch.
