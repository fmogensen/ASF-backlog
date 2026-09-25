# A wave launched 7 sessions against a fair share of 6

A product's Capacity row read "sessions 7/6" after the 00:49 wave on 2026-09-26: the wave launched 7 against a fair share of 6. Check whether the fair-share count (asf/capacity.py, and step_wave's gated_plan and the wave loop) ignores sessions launched earlier in the same wave, or counts a harvested slot as free twice. The wave must never exceed its share. Test: a wave with N free slots launches at most N, counting its own launches.

## Question
Which Epic is this under? No open Epic shares a title word with it.
