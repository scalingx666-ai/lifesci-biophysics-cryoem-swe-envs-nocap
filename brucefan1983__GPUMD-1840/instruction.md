# Issue

**Title:** main_nep refactor 14

## Description

### Summary
Reduce duplicated code in qNEP, NEP-vdW, and NEP-charge-vdW.

### Changes
- Extract shared long-range helpers.
- Split model initialization and evaluation into smaller functions.
- Reuse descriptor, output-clearing, and ZBL helpers.

### Validation
24 regression cases passed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3793896a5304`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.