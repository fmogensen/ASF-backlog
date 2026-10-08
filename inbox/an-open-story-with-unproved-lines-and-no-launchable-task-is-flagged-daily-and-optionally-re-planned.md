# An open Story with unproved lines and no launchable Task is flagged daily and optionally re-planned

Parent: E-0001

An open Story with no launchable Task stalls silently. When a decided Story's acceptance lines have no open Task to build them (all Tasks closed, retired, or never minted), the Story waits with no row. The groom digest counts "Stories without Tasks after plan-approved" but nothing acts. Asked 2026-10-08 by botseon for its parity Epic.

## Acceptance
- A daily check (groom or a tick step) lists every open, decided Story with unproved acceptance lines and no open Task, per Epic, and shows it in `asf status` and doctor. Tested.
- Under `feeder.story_gap: replan` (opt-in), such a Story queues one replan of its Feature, scoped to the unproved lines, instead of only being flagged. Tested.
