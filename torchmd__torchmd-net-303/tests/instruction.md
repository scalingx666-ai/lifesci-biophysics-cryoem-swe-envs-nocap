# Issue

**Title:** Add SPICE 1.1.4, remove leftover remove_ref_energy in some datasets

## Description

- Remove bogus instances of the old remove_ref_energy.
- Added SPICE 1.1.4 to spice.py. The ET example is referencing this version, but the dataset does not allow it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2429c94c9588`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.