# Issue

**Title:** update hdf5 dataset

## Description

Update hdf5 dataset such that now charges, spin and partial charges can be included at the same time. Also, renamed partial charges to 'pq' to be consistent with other datasets such as Ace.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `166b7db86616`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.