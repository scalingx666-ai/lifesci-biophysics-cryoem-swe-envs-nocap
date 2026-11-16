# Issue

**Title:** fix: preserve phase provenance in direct analysis

## Description

## Summary
- Preserve completed phase records when a direct single-phase command rewrites `manifest.json`.
- Record the current direct CLI phase result before manifest serialization.
- Preserve phase ordering, replace duplicate historical phase names with the latest record, and recover from malformed phase/option shapes.
- Keep the existing `energy.csv` thermodynamics discovery fix from `main` covered without duplicating it in this PR.

## Test plan
- Focused CLI/orchestrator/thermodynamics regression tests: 10 passed.
- Full suite: 3130 passed, 82 skipped, 105 deselected.
- `git diff --check`: passed.
- Added-line security scan: no hardcoded secrets or unsafe execution patterns.

## Context
This addresses the provenance loss observed when running `fastmdx analyze` against a completed simulation output. Private validation artifacts and `.hermes/` records are not included.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84126a4b9d19`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.