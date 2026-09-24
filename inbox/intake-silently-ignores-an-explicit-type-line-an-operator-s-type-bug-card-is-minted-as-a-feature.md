# Intake silently ignores an explicit type: line — an operator's 'type: bug' card is minted as a Feature

An inbox card that says `type: bug` (or any `type:` line) is silently minted as whatever its shape reads as. A card without a `signature:` becomes a Feature, so an operator filing an urgent S1 Bug got a Feature on the first pass. Reported by the first customer install.

Fix, one of two, deterministic:
- An explicit `type:` on an operator-filed card wins, and the shape rule fills only what's missing. A Bug without a signature gets one derived from its title.
- Or intake refuses the card loudly: it stays in the inbox, with a groom fix line "type: bug needs a signature: line", and is never minted as the other type.

Test: `type: bug` without a signature → a Bug, or a clear refusal; never a Feature.
