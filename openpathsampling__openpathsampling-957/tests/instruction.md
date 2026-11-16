# Issue

**Title:** Fix so SimStore tests pass without simtk.unit

## Description

Without `simtk.unit` installed, SimStore wasn't working. We only test SimStore with full integrations (similar problem to #954).

This also adds an `extras` for SimStore, so (in future releases) `pip install openpathsampling[simstore]` will install the extra requirements for SimStore as well.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a681c17e195`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.