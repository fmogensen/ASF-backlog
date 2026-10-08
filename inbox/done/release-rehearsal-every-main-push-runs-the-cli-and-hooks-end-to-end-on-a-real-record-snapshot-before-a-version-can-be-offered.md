→ F-0306

# Release rehearsal: every main push runs the CLI and hooks end to end on a real-record snapshot before a version can be offered

Parent: E-0001
priority: need

Release rehearsal on real data. ASF's tests use tidy fixtures, so defects that only show on a real product's record and hooks (5-digit ids, staged-guard vs groom, hooks pinned to a stale binary, annotated tags) reach production. They're found there, one per hour or two on botseon, and the 2026-10-08 staged-guard regression shipped in the hardening pass itself.

## Acceptance
- A CI job, `rehearsal`, runs on every main push before the version is offered to any product. It takes a scratch copy of a real product record (a sanitised snapshot committed or fetched as a CI artifact, with names redacted) plus the sample product, and runs against it end to end:
  - `asf groom --apply` with answered inbox cards;
  - `asf new` (task, story);
  - `asf set` across the settable fields;
  - `asf undeliver`;
  - `asf index` and `asf check`;
  - the record pre-commit and pre-push hooks (redact plus staged-guard);
  - `asf upgrade --to <tag>` (dry run);
  - `asf tick --dry-run`.
  Each step must exit 0 with the record still passing `asf check`.
- The release step (the tag) and `asf upgrade --product` refuse a version whose rehearsal didn't pass. Tested.
- The snapshot refreshes weekly, and a failing rehearsal names the step and the command.
