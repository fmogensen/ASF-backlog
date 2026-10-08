# Release channels: edge (ASF itself, every green tag) and stable (weekly, after rehearsal plus 48 h on edge with no S1)

Parent: E-0001
priority: need

Stable-core plan, item 10. Release channels.

## Acceptance
- `asf upgrade --channel edge|stable`. Edge is the newest green tag. Stable is the newest tag that passed the release rehearsal and has run 48 h on the edge product (ASF itself) with no S1 filed. Each product's config names its channel (default stable). Tested.
- A weekly stable promotion is computed by code and logged. `asf status` shows each product's channel and version. Tested.
