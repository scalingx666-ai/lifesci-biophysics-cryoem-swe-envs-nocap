# Issue

**Title:** Fix RMSD externs to be compatible with Cython 3.0.

## Description

* We needed this fix internally to make mdtraj work with Cython 3.0.
* This fixed it, however, we build rdkit ourselves using Blaze (Bazel), so this fix might not be applicable upstream.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6d77f4eeb398`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.