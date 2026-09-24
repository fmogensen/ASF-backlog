# conventions.doc_paths: let a product declare extra document folders so approved spec branches land without a session

Native docs-only landing counts only specs_dir, plans_dir and reviews_dir as documents. A spec branch that also touches the product's own decision register (or another pure-docs folder) is therefore "not docs-only". Instead of merging directly once green, it needs a full spec session just to land an already-approved spec. On the first customer install, 10 approved branch-only specs each need an Opus session for this.

Want: `conventions.doc_paths`, extra globs a product declares as documents (e.g. its decision register, its docs/). They count for docs-only landing and for the doc lane, and never for code gates. The default is empty, so nothing changes until a product sets it. doctor lists them.

Tests: a branch touching specs_dir plus a declared doc_path → docs-only and merged natively; an undeclared path → not docs-only.
