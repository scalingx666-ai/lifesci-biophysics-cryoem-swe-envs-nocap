# Issue

**Title:** Fix cs

## Description

This pull request updates the version to 1.4.7 and significantly refactors the composition transformation logic to use element names (symbols) instead of numeric types throughout the `calphy/composition_transformation.py` module. The changes improve clarity, robustness, and correctness of atom mapping and transformation, especially for systems with complex or edge-case compositions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4489abee7641`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.