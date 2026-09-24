# Flow-style config maps load as strings; readers silently fall back

signature: "flow-style map in product config loads as a string; landing_checks_missing silently falls back to local-gate"
severity: S2

## Description
- A product file value written as a flow map, `landing_checks_missing: {docs: wait, code: wait}`, is loaded by the config reader as the literal string `'{docs: wait, code: wait}'`. Flow lists `[a, b]` load correctly.
- `harvest.missing_policy` receives a non-dict value that is not `wait` and returns local-gate for every landing class. There is no error or warning, so the product silently gets the opposite of what it configured.
- The same shape (`conventions.models: {review: light}`) crashed the wave with `'str' object has no attribute 'get'`, so this is one defect class: shape-unsafe readers over a loader that does not parse flow maps.

## Repro
Put the flow-map value above in a product file, load it with `env.load_product(<product>)` plus `preamble.conventions`, then call `harvest.missing_policy(conv, 'docs')`. It returns local-gate.

## Expected
Either the loader parses flow maps, or `doctor`/`check` rejects any config value whose shape does not match what its reader expects. A shape mismatch must never silently fall back.

## Workaround
Use a scalar (`landing_checks_missing: wait`) or block form.
