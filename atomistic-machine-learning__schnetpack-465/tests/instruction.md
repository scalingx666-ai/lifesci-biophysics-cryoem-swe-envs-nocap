# Issue

**Title:** Make return of vector representation in SO3net optional

## Description

Output of the Cartesian vector representation in the `SO3net` representation can now be controlled with the  `return_vector_representation` option.

Returning the Cartesian vector representation by default can lead to unwanted memory overhead, especially for bulk systems. Due to this, `return_vector_features` is now disabled by default in the `so3net` config.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02bf00b6ea25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.