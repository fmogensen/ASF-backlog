# Adopting a record: flag cards that belong to another product and move or remove them in bulk (asf move)

When ASF is adopted by a product whose record already holds items that belong elsewhere (another product's cards, factory machinery, migration fragments), nothing flags them. They sit in the product's queue, can hold tier-2 rows (an S1 gate), and clearing them takes hand-edited `removed:` / `moved_to:` lines per card. Reported by the first customer install (dozens of cards).

Want:
1. The groom flags "this card looks like it belongs to another product": its paths or terms match another registered product's scope, or ASF's own. It shows them as one decision line, not one per card.
2. `asf move <ids…|--query> --to <product>|--remove "<reason>"` writes `moved_to:` / `removed:` on every card in one commit through the record's parser, and for a move imports the cards into the target's inbox, deduped.
3. An S1 item in a record that isn't the product's own work is surfaced as a NEEDS DECISION line naming `asf move`.

Tests: bulk remove writes each card and makes one commit; a move imports to the target inbox deduped; the groom flags a foreign card.
