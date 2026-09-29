# Sessions die with 'dead pid' 5.5 times a week
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 5.5 runs/week (threshold 5). 11 runs ended 'dead pid' without landing over 14 days, $7.41 and 3.3 h spent on them; threshold 5/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: failure:dead pid #1

## Acceptance
- [ ] failure:dead pid reads below 5 runs/week over 2 weeks after landing
