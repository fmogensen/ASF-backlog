# Acceptance coverage, test strength and a human proof class for Story lines
parent: E-0006

type: feature
parent: E-0006

Acceptance coverage, test strength and a `human` status for Story lines (from the botseon exchange, 2026-10-11).

## Problem
ASF proves a Story line with a `Proves: S-x line N — <test>` trailer (asf/proves.py, F-0040), and "no test, no done" holds. But no code checks that a Feature's or spec's lines cover all its capabilities, or that the proving test really asserts the line; only LLM reviews judge both. A line only a human or a live system can verify has no status, so its Story stays open forever.

## Candidates
1. A `human` proof class: `Proves: S-x line N — human: <how verified>`, reported apart from test-proved. Botseon's data: 1 of 48 Stories needed it, so classify strictly.
2. A test-strength check: the named test must exist on main, run in CI and assert something the line names (code first, LLM only as fallback).
3. Coverage: each spec capability maps to at least one acceptance line, and no empty or vacuous line.

## Acceptance
- Each candidate either ships with tests or is recorded here as measured unnecessary on ASF.
