# asf next/status read backlog_dir while the tick acts on its own clone — NEXT lags the tick

Proven 2026-09-26: `asf next` and `asf status` read `product.backlog_dir` (~/Code/botseon/backlog, asf/feeder/render.py:100). The tick's wave reads its own clone (~/.ASF/state/<product>/record, asf/tick/tick.py:164). At 12:40, B-1380 appeared 3 times in the tick's index and 0 times in backlog_dir. So NEXT can lag the tick by a full cycle plus a pull, and operators and peer sessions read a stale queue. That happened twice today: B-1378 and B-1380 looked "missing".

Expected: next and status read the same record the tick acts on (the state clone), or pull it first and print the record sha they read.

Test: a card typed into the tick's clone shows in next without a manual pull.
