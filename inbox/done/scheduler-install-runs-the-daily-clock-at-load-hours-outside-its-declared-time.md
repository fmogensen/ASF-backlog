→ B-0115

# scheduler install runs the daily clock at load, hours outside its declared time
signature: scheduler install: a calendar clock declared for 06:50 started a run at load time 03:20
parent: E-0001
severity: S3

On 2026-09-24 at 03:20 CEST, `asf-live scheduler install --product botseon` bootstrapped asf.botseon.tick, .batch and .daily, and all three started a run at once. The daily clock is declared for 06:50, but it ran at 03:20 on load, which means a groom/daily pass outside its window, in parallel with the first tick and batch.
Expected: a timed (calendar) clock does not run at load; only interval clocks may. Or install says which jobs it starts now.
