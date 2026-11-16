# Issue

**Title:** test GROMACS 2023.1 (and use Py 3.11 for the all version test)

## Description

- test GROMACS 2023 in CI matrix (#251)
- use Python 3.11 as the primary python version for testing all GROMACS versions
- fix tests for replacement of do_dssp with dssp (#249)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2b6344e347a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.