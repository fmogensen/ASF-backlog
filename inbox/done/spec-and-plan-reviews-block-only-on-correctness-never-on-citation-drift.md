→ S-157259

# Spec and plan reviews block only on correctness, never on citation drift
parent: F-0346

type: story
parent: F-0346

Spec and plan reviews block only on correctness. Measured 2026-10-11: spec and plan sessions were 273 of 1471 launches (19%); F-0334's plan and F-0340's spec each took 4-5 rounds, mostly on citation drift (line numbers, base hashes, counts) caused by main moving under the branch.

## Acceptance
- The doc-review brief classifies each finding as correctness (a Task's code, acceptance or Gate would be wrong) or drift (a citation, line number, base hash, count or wording). Only correctness findings make the verdict `changes`, and drift goes into a follow-up note (test over the brief and the verdict parser).
- A review verdict whose findings are all drift is applied as approve, with the notes kept (test).
