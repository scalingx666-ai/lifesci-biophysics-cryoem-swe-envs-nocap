# Issue

**Title:** do unit conversion first for precision

## Description

## Description
Looking into the error on `master`, I noticed that the temperature you got from the integrator was `269.99999999999994` instead of the expected `270`, so I combined the unit conversion factor to keep some extra precision. This solves the issue the master tests have been having

## Todos
- [X] Implement feature / fix bug
- [NA] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [NA] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [X] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Status
- [X] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `04620635ccda`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.