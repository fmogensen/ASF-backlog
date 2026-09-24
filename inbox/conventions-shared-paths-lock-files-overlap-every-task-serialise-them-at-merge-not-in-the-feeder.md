# conventions.shared_paths: lock files overlap every Task — serialise them at merge, not in the feeder

A lock file (the package manager's lockfile, a generated schema, a migration index) is touched by almost every Task that adds a dependency. As a `writes:` path it overlaps nearly every other Task, so the feeder serialises unrelated Tasks and widen_footprint refuses to add it ("widening +<lockfile> overlaps T-…"). Reported by the first customer install.

Want: `conventions.shared_paths`, globs the product declares as merge-serialised shared files.
- They never count toward footprint overlap in the feeder, widen or `asf check`.
- Harvest serialises them at merge instead: branches touching the same shared path land one at a time, each re-gated on the new trunk, with a regenerate step if the product declares one (`shared_regenerate: <cmd>`, e.g. the install command that rewrites the lockfile).
- The default is empty.

Tests: two Tasks sharing only a shared path → both launch; harvest lands them one after another; widen adds a shared path freely.

## Question
Which Epic is this under? No open Epic shares a title word with it.
