# Issue

**Title:** Hard contacts

## Description

Added  two versions of hardContacts() function : One where the distance calculated between centroids is 3D Euclidean distance where for other it is 2D Euclidean distance. Useful if someone wants to find specific AG neighbors around a given AG selection within a cutoff radius. Also, useful if one wants to calculate local density of one AG around another within a cutoff radius.

Added 2D version of logisticContacts() earlier added by @agrossfield.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36c0596aa980`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.