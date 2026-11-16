# Issue

**Title:** Simplify Hydrophobic SMARTS without changing meaning

## Description

- Hydrophobic SMARTS was harder to read than necessary
- Tests were not covering possible divergence between implicit and explicit H mols

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99f3ced12838`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.