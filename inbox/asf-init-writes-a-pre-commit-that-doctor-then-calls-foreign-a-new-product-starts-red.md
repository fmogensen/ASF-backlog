# asf init writes a pre-commit that doctor then calls foreign — a new product starts RED

`asf init` writes a `.githooks/pre-commit` into a new record that runs `asf check`. doctor's redaction-hooks check then treats that same hook as foreign, because it lacks `asf redact --pre-commit`. So a freshly initialised product starts RED on ASF's own hook. Found while writing the user guide.

Fix: the hook `asf init` writes also runs `asf redact --pre-commit` (and there's a matching pre-push), so the product starts green. Test: init, then doctor → redaction-hooks ok.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
