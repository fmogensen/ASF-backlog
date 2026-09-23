# A decided Epic with no Features never starts: no row breaks an Epic into Features
parent: E-0002

No wave row breaks an Epic down into Features. E-0004 (the control plane, the web UI) was decided on 2026-09-21. It has a full description and a prototype link, but zero Features, so no tick will ever start work on it. E-0002 and E-0003 are the same. The feeder's rows (asf/feeder/rows.py) begin at CARD → SPEC, and a card is a Feature. Nothing proposes the Features of a decided Epic.

Expected: an EPIC → FEATURES row. For a decided Epic with no open Features, one session drafts its Feature cards from the Epic's description. They go through the inbox or the groom, so ranking and approval still apply. The session is gated like other groom-derived work, and the Epic's rank orders it.
