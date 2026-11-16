# Issue

**Title:** Gromacs 2018 and pytest low-performance custom plugin to control -nt

## Description

~Include Gromacs 2016 in tests ()~

EDIT: We use Gromacs 2018 (see PR #127 and discussion in #130 ). This PR contains experimental pytest plugin to provide the `pytest --low-performance` flag and code to explicitly set the number of threads for `gmx mdrun` in the tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9416e076bb4a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.