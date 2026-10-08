# CI ci-job:notes is red 6 times a week
parent: E-0001

Filed by the scorecard loop for product asf (product cause, last 14 days).

Reading: 6 red runs/week (threshold 3). 12 of 639 runs red over 14 days, 2 runner minutes; threshold 3/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: ci-job:notes #1

## Acceptance
- [ ] ci-job:notes reads below 3 red runs/week over 2 weeks after landing
