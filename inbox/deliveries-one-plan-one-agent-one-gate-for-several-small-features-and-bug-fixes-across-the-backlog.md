# Deliveries: one plan, one agent, one gate for several small Features and Bug fixes across the backlog
parent: E-0002

Feature, parent E-0002. Decided by the operator on 2026-09-23 ("wouldn't it be more effective to write a plan covering several smaller features or bug fixes into a bigger delivery done by one agent with much less overhead?"). Ranked with the speed/cost work.

**Why.** Every item pays a fixed cost before its code: a Feature pays a spec plus a plan on Opus (about $2.11 + $2.98), then one session per Task. Each session loads the repo and brief, runs the gate and harvest, and about 28% of sessions end failed and cost a correction. The code is the cheap part: $0.98 per coder session and $0.48 per fix-bug session. F-0031 took 43 sessions for 6 Tasks. F-0086 batches Stories only inside one Feature. Nothing combines small items across Features and Bugs.

**What.** A *Delivery*: one plan, one branch, one coder session, one gate and one landing for several small decided items.
- **Selection (rule, no model):** decided items below a size floor (small Features, S2/S3 Bugs, Stories marked small), with the same area or overlapping `writes:`, up to a budget (N items, M files, a token estimate).
- **One plan session** writes a spec-lite section per item (its acceptance) and one Task list. No separate spec and plan per small item.
- **One Sonnet coder** builds it in item order, one commit per item naming the item. The gate runs once.
- **Harvest** lands it once, and each item closes by its own commit, so evidence stays per item.
- **Failure policy:** if one item fails its acceptance, it is split back out as its own row. The rest still land.
- **Parallel:** large or unrelated items keep their own lanes. The wave runs deliveries alongside them, and footprint rules still apply.

**Measured:** cost per landed item, sessions per landed item, time to land and failure rate, delivered vs. single, on this product. The rollup reports it.

**Relation:** it builds on F-0086's batch signal and F-0041's size classes. Once built, it replaces "batch" within a Feature as the special case of a single-Feature delivery.
