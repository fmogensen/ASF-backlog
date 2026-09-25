→ F-0144

# asf set cannot widen a Task's writes: footprint

Widening a Task's writes: after a push refusal names a file outside it (botseon T-0338, 2026-09-25) is a routine fix. `asf set T-0338 writes=[...]` refuses with "task has no settable field 'writes'", so the botseon session edited the authored line by hand. Make writes: (and after:) settable through asf set, with the same index refresh and stage asf set does for other fields.
