# Batch size by path class (light-path batches larger), configurable

Parent: E-0001

Batch size by path class. A product wants larger merge-queue batches when every member is a light-path PR (package-only or test-only paths) and smaller batches otherwise. `conventions.merge_queue.batch_size` is a single number today. Asked 2026-10-08 by botseon, with operator approval for parity speed.

## Acceptance
- `conventions.merge_queue.batch_size` accepts `{default: N, light: M}`, where `light` applies when every member's changed files match `conventions.merge_queue.light_paths` (globs). A plain number keeps today's meaning. Tested.
- The cut logs the class it used ("batch of 6 (light)"). A mixed batch uses `default`. Tested.
- doctor validates the shape. Tested.
