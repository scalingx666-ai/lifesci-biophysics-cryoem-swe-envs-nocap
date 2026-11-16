# Issue

**Title:** Simplify strings in conventions for pure functions

## Description

The character for the angular momentum is omitted because it is redundant. With this change, the conventions are no longer limited in angular momentum by the availability of alphabetical characters.

Some mistakes in the docstring of `MolecularBasis` are also fixed. More elaborate documentation will be added in a separate PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58d5e4bd1fb8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.