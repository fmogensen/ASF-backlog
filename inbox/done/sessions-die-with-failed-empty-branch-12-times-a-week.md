→ F-0157

# Sessions die with 'failed: empty branch' 12 times a week
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 12 runs/week (threshold 5). 24 runs ended 'failed: empty branch' without landing over 14 days, $14.42 and 8.9 h spent on them; threshold 5/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: failure:failed: empty branch #1

## Acceptance
- [ ] failure:failed: empty branch reads below 5 runs/week over 2 weeks after landing
