# Issue

**Title:** Convert RadiallySymmetricRestraint Forces to CV Forces

## Description

This is my implementation of the CV Force conversion of the `RadiallySymmetricRestraintForce` class and subclasses. The call to the parent class is slightly different, the parent class is a `CustomCVForce`, and subclasses are no longer `CustomBond-` or `CustomCentroidBondForce`s.

These tests pass locally but the implementation should still be reviewed. 

While waiting for a review, I will work on the YANK side of things to take advantage of these changes.

 - [x] Implement feature / fix bug
 - [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e803f50e28fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.