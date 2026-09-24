→ B-0103

# asf init writes a pre-commit that doctor then calls foreign — a new product starts RED
signature: doctor redaction-hooks is RED on a freshly initialised product - the pre-commit asf init writes does not run asf redact --pre-commit
parent: E-0001
severity: S2

`asf init` writes a `.githooks/pre-commit` into a new record that runs `asf check`. doctor's redaction-hooks check then treats that same hook as foreign, because it lacks `asf redact --pre-commit`. So a freshly initialised product starts RED on ASF's own hook. Found while writing the user guide.

Fix: the hook `asf init` writes also runs `asf redact --pre-commit` (and there's a matching pre-push), so the product starts green. Test: init, then doctor → redaction-hooks ok.
