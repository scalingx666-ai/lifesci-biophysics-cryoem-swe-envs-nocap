# Issue

**Title:** Support for NPT unwrapping

## Description

Add support for unwrapping NPT simulations. The TOR scheme outlined in https://doi.org/10.1021/acs.jctc.3c00308 is used.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f536955909c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.