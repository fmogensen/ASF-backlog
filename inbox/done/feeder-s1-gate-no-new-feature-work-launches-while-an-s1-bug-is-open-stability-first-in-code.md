→ F-0307

# Feeder S1 gate: no new Feature work launches while an S1 Bug is open (stability first, in code)

Parent: E-0001
priority: need

Stable-core plan, item 3. While any S1 Bug is open for a product, the feeder launches no new Feature work for that product. Bug fixes and ranked stability Features run first.

## Acceptance
- Under `feeder.s1_gate: on` (the default for 1.0), feature rows are held with "WAITS ON S1 <id>" while an open S1 Bug exists for the product. Bug rows, and Features carrying `stability: true` or ranked in the stability set, still launch. Tested.
- `asf status` shows the gate and the S1 that holds it. Tested.
