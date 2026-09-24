# Predictive capacity: place a launch on the account that can finish it, by provider kind

## What is wanted
The account picker plans a launch against the capacity it will need, not only the capacity left now.
Worker capacity is described per provider kind in operator config: `window` (rolling windows with
reset times; a session crossing 100 % dies), `credit` (balance, refill, rate limits, price per token)
and `metered` (budget cap, rate limits). Per launch, in code: predict tokens and minutes for the row
kind x model from the session ledger (median of recent sessions); drop an account that would cross a
ceiling before the predicted end; prefer the cheapest feasible account (scarcity rising as a window
fills far from its reset); keep writer != reviewer != fixer by account; tie-break on most room.

## Evidence (reported by the first customer install)
Sessions died on a window cap mid-run; the picker is greedy per launch with no look-ahead, no reset
planning and one provider kind. F-0079 makes the ceilings configuration and F-0082 adds a cooldown
band; neither predicts a session's consumption nor plans heavy work after a reset.

## Fix direction
Extend the pool's account model with a `kind` and the prediction step above; print the chosen
account and its reason on every launch row; a credit balance under threshold is a `NEEDS OPERATOR`
line with the runway in hours; a cap death relaunches from the branch on the next feasible account.

## Test
Fixture ledger + two window accounts, one at 80 % with a far reset: a row predicted to need 30 % is
placed on the other; with both infeasible the row waits with reason `predicted cap`; a week-long
replay shows zero cap deaths.
