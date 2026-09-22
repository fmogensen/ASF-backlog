# The groom answers itself (D-0049): rules as code, an adjudicate session for the rest, a digest to the operator
parent: E-0001
type: feature

Implements D-0049 for the `asf` product (and any product whose yaml sets `approvals.groom: auto`).

## Parts

1. `asf groom --auto`: every question class in D-0049's table answered by code, the answer and its
   reason written into the groom file's `answer:` slot, then applied (`--apply`) in the same run.
   The tick runs it in the record step.
2. Judgement calls: a `GROOM → ADJUDICATE` row — one session, cheapest model, the record as context,
   answers only the lines the rules left blank, written in the same slot; never edits cards directly.
3. Approvals matrix as code (F-0031): `approvals:` in the product yaml — action classes →
   `auto | inform | human-now`; the groom refuses to answer a `human-now` line and prints it as
   `NEEDS OPERATOR`.
4. Inform: the tick's digest (F-0078's "done since last tick" table gains a "decided" block) and a
   `notify:` command in the product yaml (`notify: <command>` receives the digest on stdin) — the
   operator's channel is operator config, not the factory's business.
5. Override: a card edited by the operator within 24 h of an auto answer wins; the groom never
   re-answers a line that carries `answered_by: operator`.

## Tests

Each rule row is one test on a fixture record; the adjudicate row on the fake runtime; the notifier
with a fake command; a `human-now` line stops with `NEEDS OPERATOR`.
