# One enforced card schema: machine fields in strict frontmatter, validated on every write, invalid cards visible

parent: F-0334 (ASF 0.3). Generic.

Problem (measured 2026-10-10): record cards are markdown with a hand-parsed YAML-like frontmatter plus machine-relevant fields scattered in the body (`stories:` lines, `**Gate**` blocks, Acceptance checkboxes). Nothing enforces one schema at write time, so: 8 ASF cards became unreadable (multi-line values in one-line lists) and silently dropped out of the kernel's facts; `asf set writes="a, b"` stored "a," entries; 36/38 Tasks had empty Acceptance while tests lived in a body Gate block; botseon's record shows 645 validator errors.

Change (staged, simple):
1. One card schema, versioned, as a JSON Schema in the repo (stdlib-only validator; no new dependency). Machine fields live only in the frontmatter: id, type, state, priority, rank, parent, after, stories, writes, creates, acceptance[{line, test}], gate[commands], risk. Prose stays in the markdown body for humans.
2. Frontmatter is parsed strictly (JSON frontmatter, or a strict YAML subset with a round-trip guarantee); a value that does not round-trip is refused.
3. Enforced at every write: the record writer refuses an invalid card; a record pre-commit hook and a CI check refuse invalid cards from any writer (sessions, hand edits, other tools).
4. The kernel reports an invalid card as LIMBO with the schema error, never drops it silently.
5. A migration (`asf record-repair --apply`) moves body machine fields into the frontmatter and fixes existing cards; run it on each product's record before it moves to the kernel.

Acceptance: the schema file exists and is validated by tests; writing an invalid card through any path fails with the schema error; the kernel lists invalid cards in LIMBO; record-repair on a copy of today's ASF record yields 0 schema errors.
