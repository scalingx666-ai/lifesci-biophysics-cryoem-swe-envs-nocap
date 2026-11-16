# Issue

**Title:** [READY] Create visualization module for generating PyMOL movies

## Description

Visualization module for perses protein mutations and small molecule transformations

TODO
----

- [x] Add test for protein mutations
- [x] Test on xtc ~~and PDB~~ trajectories
    - Note: PDB trajectories are too slow in their read/write times
- [x] Add test for small molecule transformation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59f47eccb757`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.