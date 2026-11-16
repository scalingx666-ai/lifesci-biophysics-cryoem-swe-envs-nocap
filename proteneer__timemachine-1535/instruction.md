# Issue

**Title:** Fix REST heuristics

## Description

Fixes 2 separate bugs in the REST heuristics introduced in #1524.

1. Fixes the implementation of `target_torsions`. Previously, the filter predicate was trivially true due to a subtle issue with the implementation using `dataclasses.astuple`.
2. Refines REST softening of ligand-ligand nonbonded interactions to only include pairs with at least one atom in the REST region. (Previously all pairs were softened.)

TODO
- [x] Update result hashes in nightly tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51248370bdd3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.