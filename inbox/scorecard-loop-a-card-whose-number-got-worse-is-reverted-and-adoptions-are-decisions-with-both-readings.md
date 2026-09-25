# Scorecard loop: a card whose number got worse is reverted, and adoptions are Decisions with both readings
parent: E-0001

Feature: the rest of F-0047's loop. The scorecard loop files one card per cause over threshold and, N weeks after the card lands, records "moved" / "didn't move" on it and reopens a card that didn't move with both numbers (asf/scorecard/loop.py verify).

Still missing: a change that made its cause's number *worse* (after > before by the same min_move) is reverted — the loop opens a revert card naming the landing commit(s) of the card, with the two readings, and records the adoption / reversion as a Decision with the readings behind it.

## Acceptance
- [ ] a fixture where the number rose after landing produces a revert card naming the commits and both readings
- [ ] a moved card writes a Decision with the two readings
