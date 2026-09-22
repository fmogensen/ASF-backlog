→ F-0084

# The command surface reads as one product: names, one voice, and "writes" marked
parent: E-0001
type: feature
## Description
As a stranger who just installed `asf`, I want `asf --help` to tell me in one line what each
command does, whether it writes anything, and in one consistent voice — so that I can find the
right command without reading the code.

Evaluated 2026-09-21 19:30 over the 30 commands. Findings: two commands whose names disagree with
what they print or with each other (`migrate` is the importer of goals/specs, colliding with
`schema-migrate`; `backlog` prints the BOARD); one carries the first product's concept in its name
(`parity`); three descriptions name a path or internal (`stale` → tools/limits.json, `hook` →
tools/checks, `evidence` → "the evidence pass"); the voice mixes imperative, noun phrase and a
question; no description says which commands write the record.

## Acceptance
- [ ] Renames: `migrate` → `import` (the one-time adopter of Epics/Features/Stories from a repo's
  goals, specs and plans); `backlog` → `board` (the BOARD table) with `backlog` kept as an alias
  for one release; `parity` → `stories` (one row per Story; PARITY was a product's word).
  `schema-migrate` loses its "(not `migrate`…)" parenthesis.
- [ ] One voice: views are noun phrases ("the ROADMAP table — one row per Epic"), actions are
  imperatives ("land finished worker branches"), no questions.
- [ ] Every command that writes says so at the end of its line: "(writes the record)",
  "(writes ~/.ASF)", "(launches sessions)"; the rest are read-only by construction.
- [ ] Revised lines for: `check` (ids, parents, links, typed fields, a fresh index), `ingest`
  (derive every item's state from the repo: branches, PRs, commits, CI — writes the record),
  `groom` (type inbox items, apply yesterday's answers, write today's questions — writes the
  record), `stale` (items past their stage limit from the product yaml), `evidence` (the raw
  evidence the ingest reads — a debugging view), `harvest` (finished, green, pushed → rebased and
  landed; never a hand merge), `hook` (run one rule's hook by rule id), `tick` (one pass of the
  factory: record → health → wave → prs → batch → daily), `new` (a Bug or a Decision; Epics and
  Features come from the groom).
- [ ] `--help` output is a golden test; a new command without a description fails it.
- [ ] `plugin/` skill names follow the renames.

## History
- 2026-09-21 19:30 operator: "evaluate the description on all asf commands … some might need
  revision"
