# Issue

**Title:** ChemComp#monNstdParentCompId now returns null instead of empty String

## Description

ChemComps without parent would erroneously return an empty String. This restores the old behavior and will return `null`.
This .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53c2dc0c3807`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.