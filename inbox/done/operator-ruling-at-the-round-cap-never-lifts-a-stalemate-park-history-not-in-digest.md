→ F-0239

# Operator ruling at the round cap never lifts a stalemate park (History not in digest)
parent: E-0001

Seen 2026-10-04 on botseon T-0338: STALEMATE -> ADJUDICATE shows "PARKED adjudicated, card unchanged" although two binding operator rulings (asf correct at the round cap, 10-03) are on the card History. The park digest ignores History, so an operator ruling never lifts the park, and `asf unpark T-0338` answers "not parked". Fix: an operator ruling counts as a card change for the stalemate park check (or asf correct at the cap clears it).
