# check: bare decision reference fires inside fenced code (byte-identical acceptance tests)

`asf check` reports "bare decision reference" for tokens like `D287` that sit inside fenced code
blocks of a Task body — e.g. an acceptance test copied from the spec:
    describe('setup mode (D287 b)', () => { … })
The plan contract requires those acceptance tests to be byte-identical to the spec, so the reference
cannot be rewritten to `[[D-0287]]` — and here `D287` names the product repo's own decision register,
not a record D-card. Six such findings in one product block every commit once the pre-commit runs
`asf check`.

Expected: the bare-reference rule skips fenced code blocks (``` … ```) and inline code spans.
Acceptance: a check test with `D287` inside a fenced block yields no finding; the same token in prose
still does.

## Question
Which Epic is this under? No open Epic shares a title word with it.
