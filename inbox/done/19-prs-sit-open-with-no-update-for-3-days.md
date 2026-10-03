→ F-0236

# 19 PRs sit open with no update for 3 days
parent: E-0001

Filed by the scorecard loop for product asf (factory cause, last 14 days).

Reading: 19 PRs (threshold 10). 19 of 35 open PRs are stale; threshold 10. The factory reaps what it opened.

Verify: the loop reads this number over the 2 weeks before this card lands and the 2 weeks after; it must fall by 20 %, or the card is reopened with both numbers.

scorecard-cause: clutter:stale-prs #1

## Acceptance
- [ ] clutter:stale-prs reads below 10 PRs over 2 weeks after landing
