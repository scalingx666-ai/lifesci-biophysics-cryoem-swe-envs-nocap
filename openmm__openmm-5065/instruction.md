# Issue

**Title:** Fix error message formatting for bonds to elements without atoms

## Description

Fixes a minor bug in #5020 in which error messages for incorrect bonds to elements without atoms don't get formatted and an unrelated error gets shown instead.  Normally, there shouldn't be bonds to extra sites that would trigger this, but it can happen if one attempts to parameterize a topology with explicitly specified bonds to atoms for which OpenMM couldn't guess the elements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8798cb3db9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.