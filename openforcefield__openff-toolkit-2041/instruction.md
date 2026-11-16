# Issue

**Title:** Drop testing on Python 3.10

## Description

Bumps minimum Python version in tests to 3.11 following NEP 29.

Where 3.10 was one of many versions used in a matrix, it is removed.
Where 3.10 was the only version used in a workflow, 3.11 is used instead. This could have produced failures but nothing is coming back. (I did not look through logs for warnings.)

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca7231bc274f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.