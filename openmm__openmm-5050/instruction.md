# Issue

**Title:** NBFIX in Gromacs for 1-4 pairs

## Description

Code fix to match Gromacs in applying NBFIXes to 1-4 pairs when there are no explicit (1-4) pair parameters given via 'pairtypes'.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1b6236a8cc46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.