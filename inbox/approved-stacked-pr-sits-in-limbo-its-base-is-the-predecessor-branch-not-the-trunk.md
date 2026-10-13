# Approved stacked PR sits in LIMBO: its base is the predecessor branch, not the trunk
parent: F-0348

Type: Bug

A stacked PR whose review approved is put in Landing and reported as LIMBO ("green with auto-merge on, not merged after 18m ... waits on GitHub auto-merge"), although its base is the predecessor's branch, not the trunk, and auto-merge is not set on it. Observed 2026-10-13: T-166582, PR #1680, base worker/T-166581, while the predecessor T-166581 (PR #1678) was still on a fix round.

Acceptance:
- An approved stacked item waits in a named state ("waits on predecessor <id> to land") and is not counted as LIMBO or as a merge breach.
- When the predecessor lands, the kernel retargets the stacked PR to the trunk, waits for its checks on the new base, and lands it. It never merges a stacked PR into the predecessor branch and then counts the item as landed.
- Test: a replay with an approved stacked PR and an unlanded predecessor yields the wait, no LIMBO; after the predecessor lands, a retarget and a landing.
