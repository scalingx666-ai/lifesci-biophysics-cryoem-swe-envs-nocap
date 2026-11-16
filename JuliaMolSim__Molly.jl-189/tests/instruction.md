# Issue

**Title:** Added the force fields necessary for Calvados2

## Description

Calvados2 is a strongly coarse grained model for intrinsically disordered proteins (IDP) which requires a yukawa potential, an harmonic potential and a modified Lennard Jones potential.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9db6db3f5821`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.