# Issue

**Title:** Quaternary symmetry calculation edge-case

## Description

When computing symmetry with structural clustering for 9y9z an `IllegalArgumentException` was occurring in `QuatSymmetrySubunits`. Also added some logging improvements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e6ba37989bba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.