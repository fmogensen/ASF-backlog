# CI-red cards count cancelled runs and fixed history

The scorecard's CI-red card generator (asf/scorecard/diagnose.py, ci_red_per_week, default 3.0) minted botseon F-1128 (p1-e2e "red 21.5 times a week") and F-1129 (p1-e2e-b) from history whose causes were already fixed on main (B-1380, B-1379). A recount after the B-1380 fix: 112 Hetzner p1-e2e jobs in 11.2 h — 42 success, 69 cancelled, 1 failure (a known race with a fix pending).

Ask (from the botseon session):
1. Count only `failure` conclusions — never `cancelled` (or `skipped`) — toward the red rate.
2. Measure only runs after the newest landed fix for that job (a Bug whose card names the job / signature and is Resolved on main), so fixed history doesn't mint a card.
3. Optionally require a minimum run count or window before projecting a weekly rate from few reds (1 red in 11 h projected to 15/week).

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
