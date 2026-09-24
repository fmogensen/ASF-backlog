# coverage: a product declares references; generic reference coverage (generalises matrix_path + asf parity)

A product is often built against a reference: a competitor to match, a customer contract or RFP, a
regulation checklist, or a system being replaced. ASF should carry the mechanism generically; the
product supplies the reference's content and its own policies.

Generalise today's `conventions.matrix_path` + `asf parity` (one row per Story) into reference coverage:
1. The product declares references in its product file, e.g.
       references:
         - id: parity
           name: "<the product's label>"
           source: stories        # stories | file
           epic: E-xxxx           # when source: stories — the Epic whose Stories are the rows
           path: …                # when source: file — the old matrix_path (existing configs keep working)
           decide: need           # groom policy: a reference Story is decided by rule (deterministic)
           evidence: test         # a reference Story closes only on a named test green on the trunk
2. Each row is a Story traced to its reference (`reference: <id>`, `ref_row: "<section.row>"`);
   coverage = rows → Stories → Tasks → evidence.
3. Closing rule per reference: by the declared evidence kind (test by default), not on a merge alone.
4. `asf coverage [--reference <id>]`: rows built / tested / missing, % per section, gaps first,
   code-generated; `asf parity` stays as an alias of the first reference.
5. `decide: need` is a groom policy (code over LLM); approvals classes still guard the work; reference
   gaps feed STARVED → SPEC through the normal feeder.

Acceptance (first product's validation):
- the product configures one reference with `source: stories` over its reference Epic;
- `asf coverage` lists every row of that Epic with its state, built/tested/missing and a % per section;
- the table matches the record's Story states and the trunk's test evidence row by row (mismatches
  reported as defects);
- a gap row with `decide: need` is decided by the groom policy and gets a SPEC row with no operator step;
- an existing `matrix_path` config still renders `asf parity` unchanged.
Placement: after backfill (a feature, not in the fixes-only package).

## Plan (Part A)

Folded from a product session's research plan (2026-09-24, ASF at 74497d9), reviewed against
the ASF code. Generic only: ASF ships the mechanism; a product supplies the reference's content.

Sequencing: after the fix package (integration/fix-package) and after `asf backfill`
(inbox `backfill-reconcile-a-partly-built-product-s-history-into-the-record-asf-backfill`).
Dependency: *tested* needs ASF's own CI collection (card
`asf-collects-trunk-ci-runs-itself-ci-facts-go-stale-when-a-product-s-legacy-collector-is-retired`);
without fresh per-job trunk CI facts every `evidence: test` Story reads *built*, never *tested*.

Tasks (wave 1 -> (2 || 5) -> 3 -> (4 || 6) -> 7; every gate = full suite + check_generic +
check_conventions + `asf check`):
1. **Schema + doctor.** `conventions.references` (see PD0) with a per-entry field table
   `{id, name, source, epic, path, row_id_pattern, decide, evidence, jobs}`; unknown entry key ->
   NEEDS OPERATOR; `Product.references` with defaults filled and the `matrix_path` synthesis (PD3);
   doctor row `references` (epic not in record / path not on trunk / unknown source = red; a `jobs:`
   name absent from the CI stream = warning; no references = "not configured", never a dash).
2. **Evidence pass.** `reference_evidence()` called from `discover()`: per traced Story, one
   `git cat-file --batch-check` over `origin/<main>` for every `links.impl`/`links.test` path;
   newest trunk commit touching the cited tests; green at/after it (with `jobs:`, every named job
   `success` from the CI stream). Path cells split on `,` **and** `;` (today `_matrix_paths`,
   evidence.py:370, splits on `,` only). For `source: stories` a `path` is read by an id-only row
   reader, never `parse_rows` (which wants ten cells and reads status from cell 5).
3. **Closing + ingest.** `Ev.reference_status`; rules `reference-tested` (Closed) /
   `reference-built` (Resolved) (placement: PD5); `match_story` by `reference`+`ref_row` first, then
   `legacy_id`; evidence line (PD6); `untraced` line for a Story under the reference's Epic with no
   `ref_row`; `reference`, `ref_row` added to `_COMMON_SET` (new.py:18). Golden test: a
   `matrix_path` record ingests to the same bytes (PD2).
4. **`asf coverage`.** View over index.json: header, BY SECTION (rows/tested/built/missing/%, All
   line; several Stories on one row count as the weakest), GAPS (missing, built, untraced), DEFECTS
   (sticky-held Closed, undeclared reference, bad `ref_row`); `--reference`, `full`; generated skill;
   `asf parity` alias (PD9).
5. **Groom policy `decide_reference_need`** (PD8), in POLICIES/POLICY_NAMES order, `policy_on`
   honoured, approval bound first.
6. **`asf coverage --trace [--apply] [--ids]`** (PD10): derive `reference`/`ref_row` by rule,
   dry-run table, `--apply` via `set_typed` + one History line, idempotent.
7. **Docs.** product-config guide section References, old-keys row `conventions.matrix_path` ->
   `conventions.references[].path`, policies row, operating guide, example yaml (parsed by
   test_env), CHANGELOG, README tables list.

Design verdicts:
- **PD0 (placement, not in the source plan) — change.** `references:` goes under `conventions:`,
  not top level. v0.1.2 refuses any unknown top-level product key (env.py `check`, "is not a field
  of the product file"), so a rollback would stop the tick; `Conventions.from_mapping` keeps unknown
  keys in `extra`, so v0.1.2 tolerates `conventions.references`. This is the fix package's §9 item 7
  / R23. Typed Story fields `reference`/`ref_row` are rollback-safe already (no unknown-typed-key
  check). No `PRODUCT_FIELDS` change.
- **PD1 agree** — `<section>/<row>`, split on the last `/`: dotted sections and row ids never clash.
- **PD2 agree** — `source: file` keeps `parse_rows` + `legacy_id` + `matrix-*` byte for byte; the
  golden ingest proves "existing configs keep working".
- **PD3 change (half)** — synthesising a file reference from `matrix_path` is right; refusing the
  file on load when both are set halts the controller over a redundant key. Instead
  `conventions.references` wins, doctor shows red naming the stale `matrix_path`.
- **PD4 agree** — kinds are facts off the trunk and CI (code over facts); `test` needs the CI
  collection dependency above to be fresh; no-CI products fall back to existence like `Ev.green`.
- **PD5 change** — rules are first-match (closing.py `state_of`); placed after `tasks-resolved`,
  a Story with every Task Resolved stops at Resolved and never reaches `reference-tested`, and
  `tasks-closed` closes it on a merge alone, contradicting "not on a merge alone". Put
  `reference-tested` / `reference-built` before `tasks-*`, and have `tasks-*` not fire for a Story
  whose reference declares `evidence: test` (they stay for `merge`). The source plan's acceptance
  `test_tasks_closed_in_prod_still_wins_over_reference` inverts accordingly.
- **PD6 change** — keep the human line, but coverage must not parse prose: ingest also writes a
  machine field (`ref_status: tested|built|missing|untraced`, `ref_sha`) that flows into index.json;
  the view reads that.
- **PD7 agree** — index.json only, like every table; state vs ref_status disagree only under sticky.
- **PD8 agree** — Features are what the feeder launches; the policy decides the undecided Feature
  with an untested `priority: need` row, after the approval bound; deterministic.
- **PD9 agree** — byte-identical PARITY without references (golden), alias of the first otherwise.
- **PD10 agree** — tracing by rule from `legacy_id` (+ optional pattern) and `area`, dry-run first,
  idempotent `--apply`: a command, not a hand edit.
