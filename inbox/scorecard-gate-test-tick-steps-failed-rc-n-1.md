# CI gate:--- test_tick_steps: FAILED (rc N) is red 3.5 times a week
parent: E-0001

Filed by the scorecard loop for product asf (product cause, last 14 days).

Reading: 3.5 red runs/week (threshold 3). 7 of 7 runs red over 14 days, 69 runner minutes; threshold 3/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: gate:--- test_tick_steps: FAILED (rc N) #1

## Acceptance
- [ ] gate:--- test_tick_steps: FAILED (rc N) reads below 3 red runs/week over 2 weeks after landing
