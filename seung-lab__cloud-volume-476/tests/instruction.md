# Issue

**Title:** fix(spatial_index): correct calculation of file paths

## Description

Broke the spatial index query during the move to floating point resolution.

This fix changes how xyzrange is implemented to allow floating point steps and also return a clone of the point Vec (which should avoid certain issues anyway). It also makes Vec check for floating point arguments to determine dtype, so this change is technically backwards incompatible though I think few people will notice.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffdc1e1eda4c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.