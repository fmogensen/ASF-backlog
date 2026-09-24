→ F-0128

# approvals: decide_feature: auto — ASF decides a product's Features itself (operator instruction)

Operator instruction (2026-09-24): a product can be configured so ASF decides its Features itself —
no Feature waits for a human decision to be developed. Today `decided:` on a Feature is set only by a
human (or by the recurring-bug policy for Bugs), so undecided Features never launch and pile up as
"Decisions: N undecided".

Proposal — a new approval class in the product's approvals matrix:
    approvals:
      decide_feature: auto        # auto | groom | human-now (default human-now)
With `auto`, a deterministic groom policy (code, not the adjudicator LLM) sets `decided: true` on every
open, undecided Feature that:
  - is not removed/moved_to and has a parent Epic that is itself decided/Active;
  - is not superseded (links.supersedes / shared legacy_id) — those close instead;
  - crosses no other approval class that is not `auto` (spend_money, touch_production, touch_security,
    touch_customer_data, touch_legal, new_epic) — judged from the card's declared area/paths/links, so a
    pricing/payments/legal Feature still goes to the operator batched;
and writes a History line `decided → true (policy decide_feature: auto)`.
The same class could cover Bugs (`decide_bug: auto`) beside decide_or_close_ci_red.

Acceptance: with decide_feature: auto, a groom run decides every eligible undecided Feature, leaves the
barred ones as operator questions in one code-generated table, and the feeder launches CARD → SPEC /
STARVED → PLAN for them on the next tick.
