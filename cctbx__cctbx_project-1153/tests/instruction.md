# Issue

**Title:** Add bootstrap filter method to cctbx.xfel.merge

## Description

See Grünbein 2021, this adds subsampling with replacement of the lattices to enable creating replicates that can be individually used with structure refinement.  The average atomic positions provide an estimate of coordinate error.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `403abac739c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.