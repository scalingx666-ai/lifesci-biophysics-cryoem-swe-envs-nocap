# Issue

**Title:** Fix bug in ordering of Cartesian D functions in FCHK format

## Description

The two last ones were swapped accidentally and some tests from the old horton were not ported over, such that the tests did not fail like they should have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b7937a06c4bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.