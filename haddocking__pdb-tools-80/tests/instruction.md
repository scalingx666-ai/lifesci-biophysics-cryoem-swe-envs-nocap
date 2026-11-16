# Issue

**Title:** Added -strict option to pdb_tidy to avoid adding intra-chain TERs

## Description

Partially addresses the points raised in #70 by adding a `-strict` option to `pdb_tidy` that prevents the tool from adding TER records on chain breaks. The default behavior is to add TERs on chain breaks, is this OK @amjjbonvin ?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6ffb92cccd26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.