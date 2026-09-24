# Groom commits its own output; a foreign daily stamp never skips the native daily; new products groom by default

Three small groom and daily gaps from the first customer install:
1. `asf groom` writes groom/<date>.md and card edits but doesn't commit them, so the record stays dirty until the next tick's commit. The groom should commit its own output, like `asf inbox` does.
2. A stamp left by a pre-ASF daily job made ASF's native daily believe it already ran today and skip. The native daily should only trust its own stamp format, or doctor should report a foreign stamp.
3. `approvals.groom` defaults to off, so a new product's inbox is never groomed unless the operator knows to set it. `asf init` should write `approvals.groom: auto` into a new product yaml, and doctor should say "groom off: inbox is not groomed" when it's unset.

Tests for each.

## Question
Which Epic is this under? No open Epic shares a title word with it.
