→ F-0302

# stories is not settable on Tasks; backfill stories for Tasks minted before the 5-digit id fix

Parent: E-0001

`stories:` is not settable on a Task. `asf new task --set stories=[S-…]` and `asf set <task> stories+=S-…` are both refused (asf/record/new.py SETTABLE). Tasks minted before the 5-digit id fix (F-0285, 0.1.242), such as T-58xxx, T-59433..T-59640 and T-4733x on botseon, have empty `stories:` because plan_tasks couldn't read 5-digit Story ids. Parity and proof coverage counts under-report. Seen 2026-10-08 on botseon. Its proof harvest used a locally patched CLI to work around this.

## Acceptance
- `stories` is settable on Tasks: `asf new task --set stories=[…]` and `asf set <task> stories+=S-…` / `stories-=…`. Every id must exist in the record, and both the card and the Story's back-links are kept consistent. Tested.
- A one-off backfill (`asf backfill-stories --product P [--dry]`) re-derives `stories:` for Tasks minted from a plan whose Story ids are now readable, and leaves hand-set values alone. Tested.
