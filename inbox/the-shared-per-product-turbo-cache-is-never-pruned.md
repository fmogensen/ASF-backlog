# The shared per-product turbo cache is never pruned

Since 2026-09-26 a product's worker_env sets TURBO_CACHE_DIR to one cache directory per product under state/<p>/turbo-cache, shared by every worktree so unchanged packages are cache hits. Nothing prunes that directory, so it grows without bound.

Wanted in code:
- The health step prunes entries older than N days (default 7) or beyond a size cap (default 10 GB), oldest first, with one line per pass.
- Doctor shows the directory's size.
- Scorecard or status records the turbo hit rate, if cheaply readable from turbo's run summary, to prove the cache pays.
