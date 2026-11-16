# Issue

**Title:** Fix regression in computing overlap for low-overlap u_kln

## Description

- PyMBAR 4 is slower at computing the overlap
- Use a reduced max_iter/relative_tol (matching our default) to improve the performance

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38d7abfd6799`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.