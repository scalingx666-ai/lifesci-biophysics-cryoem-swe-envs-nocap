# Issue

**Title:** use and document conda packages

## Description

- fix #215
  - updated installation instructions
  - updated Gromacs -> GROMACS, minor doc fixes
  - update CHANGES
- update CI
  - use numkit conda package
  - only run CI once a week (close #209 )

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d570df4ce9ea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.