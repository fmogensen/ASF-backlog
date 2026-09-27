# Sessions die with 'failed: unpushed work' 7.5 times a week
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 7.5 runs/week (threshold 5). 15 runs ended 'failed: unpushed work' without landing over 14 days, $14.02 and 4.2 h spent on them; threshold 5/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: failure:failed: unpushed work #1

## Acceptance
- [ ] failure:failed: unpushed work reads below 5 runs/week over 2 weeks after landing
