# asf hooks install chains an existing git hook instead of refusing (stdin shared, one command)

`asf hooks install` refuses when a product repo already has its own git hooks ("NEEDS OPERATOR: … is not asf's — add the line"). The operator must hand-merge ASF's redact line into each hook. For pre-push, the hook must also tee stdin, because `asf redact --pre-push` consumes the ref lines that the existing hook needs. Reported by the first customer install; it needed an approval plus hand edits in two repos.

Want: `asf hooks install --chain`, the default when a foreign hook exists.
- Rename the foreign hook to `<hook>.local` and install ASF's hook, which runs `asf redact` and then execs `<hook>.local` with the same stdin (buffered once) and args.
- Idempotent. `asf hooks uninstall` restores the original.
- doctor accepts the chained form.
- The approvals class (touch_security) still applies to this install, and it's one command for the operator instead of hand edits.

Tests: foreign pre-commit and pre-push chained, both run, stdin reaches both; second install is a no-op; uninstall restores.
