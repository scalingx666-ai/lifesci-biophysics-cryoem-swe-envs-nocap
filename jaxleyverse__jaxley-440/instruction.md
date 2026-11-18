# Issue

**Title:** Fixups for having heterogenous numbers of compartments in each branch

## Description

This PR contains final fixes for having heterogenous numbers of compartments in different branches. It does the following things:
- update `index_of_loc`
- update `loc_of_index`
- remove `_local_inds_to_global` (which could easily be replaced in the __one__ place where it was used)

Replaces #439

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4bd3fe3c5962`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.