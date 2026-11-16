# Issue

**Title:** Fix missing GBn2 neck term in forces on sulfur atoms

## Description

When the HCT term of a pair was zero, the force code skipped the whole pair, including the GBn2 neck term that the energy does include. This happens for pairs with sulfur, which has a negative screening parameter in GBn2, so forces on sulfur atoms were off.

Skip only the HCT term and keep the neck term.

Add a test comparing GBn2 forces on HEWL disulfide sulfurs with finite differences of the energy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a6d816175f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.