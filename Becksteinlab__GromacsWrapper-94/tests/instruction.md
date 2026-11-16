# Issue

**Title:** enable initial tests for numkit

## Description

- pytest can run numkit/tests (only test for integration available at the
  moment)
- run test for numkit on travis before running the test for gromacs itself
- not sure if codecov will be happy...
- removed a blanket 'import *' from the numkit test

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9708647980f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.