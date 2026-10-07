# S1: a record write from a stale checkout deletes unrelated cards (a story mint deleted a filed S1 inbox card)

Parent: E-0001
severity: S1

A record write by a session deleted an unrelated card. Commit 5c58228f, "record(F-0258): new story S-59307" by a cloud session, also deleted `inbox/s1-4-digit-id-patterns-….md`, a card that had been filed and pushed minutes earlier. The session's checkout predated that card, and the commit staged the whole tree (or a stale index), so the newer card on the trunk read as a deletion. Any record write from a stale checkout can silently drop other people's cards.

## Acceptance
- `asf new`, `asf set` and every record publish stage only the paths they wrote: the card, its parent's link lines and index.json. Never `add -A` or a tree snapshot. Tested: a checkout missing a newer trunk card writes a Story and the newer card survives the push.
- The record pre-push refuses a commit that deletes a card it did not name in its subject (a delete is allowed only for `retire`/`move` subjects), and the refusal names the card. Tested.
- `asf check` (or doctor) finds cards deleted by commits whose subject names another item, and lists them with the commit for restore. Tested.
