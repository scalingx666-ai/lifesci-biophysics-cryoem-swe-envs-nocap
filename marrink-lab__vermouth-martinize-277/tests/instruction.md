# Issue

**Title:** Fix 275. Fix edgecase where it was not possible to patch only termini of a specific chain

## Description

In addition, split of the termini matching logic into a separate function.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69f3d2ddb0f1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.