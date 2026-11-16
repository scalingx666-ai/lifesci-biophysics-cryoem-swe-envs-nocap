# Issue

**Title:** Refactor chiral restraint setup to avoid repeated work

## Description

The chiral validity MCS filter added in #1356 is prohibitively slow in some important edge cases where the MCS search visits a large number of chirally-invalid leaf nodes before finding a valid one, because each invocation of the leaf filter is expensive.

One opportunity for improvement is to remove some of the substantial redundant work that is done when setting up chiral restraints for the hybrid mol in the leaf filter. Much of the expensive work is independent of the dummy group assignment, and can be precomputed outside of the MCS search.

This PR refactors bond and chiral restraint setup to reduce the time for a single execution of the chiral validity filter from 200-300 ms down to ~10 ms.

## Summary of changes
- Precompute quantities independent of core and dummy group assignment during MCS initialization
- Move assertion that hybrid mol is connected to `setup_end_state`
- Refactor `setup_dummy_bond_and_chiral_interactions` using vectorized numpy ops
- Remove `recursive_map`, replace with array indexing
- Vectorize bond and chiral atom ixn canonicalization
- Move sanity-check assertions behind `verify` flag (defaulting to True; disabled during MCS)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51b6a5dac47b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.