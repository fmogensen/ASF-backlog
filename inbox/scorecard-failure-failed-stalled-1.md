# Sessions die with 'failed: stalled' 46 times a week
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 46 runs/week (threshold 5). 92 runs ended 'failed: stalled' without landing over 14 days, $65.41 and 42.8 h spent on them; threshold 5/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: failure:failed: stalled #1

## Acceptance
- [ ] failure:failed: stalled reads below 5 runs/week over 2 weeks after landing
