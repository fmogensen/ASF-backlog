→ F-0158

# Sessions die with 'failed: not pushed' 72 times a week
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 72 runs/week (threshold 5). 144 runs ended 'failed: not pushed' without landing over 14 days, $207.34 and 52.8 h spent on them; threshold 5/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: failure:failed: not pushed #1

## Acceptance
- [ ] failure:failed: not pushed reads below 5 runs/week over 2 weeks after landing
