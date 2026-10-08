→ F-0296

# CI ci-job:install-clean-macos is red 8.5 times a week
parent: E-0001

Filed by the scorecard loop for product asf (product cause, last 14 days).

Reading: 8.5 red runs/week (threshold 3). 17 of 654 runs red over 14 days, 63 runner minutes; threshold 3/week.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: ci-job:install-clean-macos #1

## Acceptance
- [ ] ci-job:install-clean-macos reads below 3 red runs/week over 2 weeks after landing
