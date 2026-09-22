# The savings pass: the tick proposes the next cheapest win from its own numbers
parent: E-0001
type: feature

Implements D-0050. Once a day (the `daily` step), the factory reads its own metrics and files the
savings it can see, with the number attached:

| Signal in the metrics | Card it files |
| --- | --- |
| input tokens per session above the median by 2× | a preamble card for that brief kind (F-0022): the runner already knows what the session is discovering |
| gate minutes per landed change above N | a gate card: split the suite by footprint, or one gate per tick (B-0040) |
| rounds per landing above 1.3 | a brief card for the kind that rounds most: what the session had to guess |
| a step's duration above its 7-day median by 50 % | a step card naming the step |
| model spend per landed change above the median | a routing card: the cheapest model that passed this kind last week |
| a red gate signature seen 3× | a failure-class card (F-0087's list) |

Each card carries: the reading, the window, the expected move in percent, and the measurement that
will prove it. The scorecard gains a "savings" block: card, expected, actual, kept or reverted.

First measurement to build, because the record cannot yet answer it: minutes and dollars **per landed
change**, per kind (fix, spec, plan, code), from `metrics/sessions` joined to the landing.
