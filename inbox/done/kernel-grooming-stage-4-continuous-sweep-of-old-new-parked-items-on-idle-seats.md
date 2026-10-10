→ F-0337

# Kernel grooming Stage 4: continuous sweep of old New/Parked items on idle seats
type: feature
parent: E-0002

Stage 4 of the grooming design (trigger met: 142 Parked Tasks from September, 551 parked items). A continuous sweep over New then Parked, oldest first, budget kernel.groom.sweep_per_tick (default 3), only on idle seats and never ahead of a Ready launch. Each swept item gets one groom-fill verdict: retire (superseded_by), keep (stays parked) or revive (un-park + DoR). Code applies it.
Acceptance: a test shows the sweep never pre-empts Ready work and applies each verdict; the parked-age histogram is reported by asf kernel waits.
