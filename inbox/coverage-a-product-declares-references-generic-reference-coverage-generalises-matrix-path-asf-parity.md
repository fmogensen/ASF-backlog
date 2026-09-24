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
