# Issue

**Title:** Add `split_molecule`

## Description

Adds a function to split a `prolif.Molecule` into two based on a predicate function `Callable[[ResidueId], bool]`. This allows running the water-bridge fingerprint analysis without having to generate a separate file that contains only waters

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca93feecbc7e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.