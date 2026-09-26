# The TICK summary says every step is ok while the tick exits 1

2026-09-26 03:12: a product's tick exited 1 while its TICK summary line read "record ok, health ok, groom ok, wave ok, prs ok, harvest ok". The failing step wasn't named anywhere a person reads.

Wanted in code: the TICK line marks a failed step as 'FAILED (<first line of the reason>)'. The tick's exit code and the line always agree, and a test covers it.
