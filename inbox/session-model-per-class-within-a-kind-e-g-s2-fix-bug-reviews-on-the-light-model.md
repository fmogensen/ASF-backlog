# Session model per class within a kind (e.g. S2 fix-bug reviews on the light model)

The model a session runs on can be set per brief kind today (`conventions.models.<kind>: heavy|light`, asf/briefs/build.py model_for). But it can't vary by severity or item class within a kind: every review ran on the heavy model, including small S2 fix-bug reviews. Reported by the first customer, as a spend concern while capacity is tight.

Want: `conventions.models.<kind>` also accepts a map by class, e.g. `review: {S1: heavy, S2: light, S3: light, feature: heavy, default: light}`. The class is the item's severity for Bugs, and feature/task/story otherwise. model_for takes the row's item. A plain string keeps working. doctor shows the resolved table.

Tests: a map resolves by severity and by type; a string is unchanged; an unknown class falls back to default.

## Question
Which Epic is this under? No open Epic shares a title word with it.
