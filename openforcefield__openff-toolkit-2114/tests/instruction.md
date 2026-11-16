# Issue

**Title:** Speedup for molecule isomorphism checking

## Description

- [x] Fix #2035
    - This makes the reproducing case run on my machine in ~0.5 sec, compared to 11 min using the old stack 
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7febf6d5b3db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.