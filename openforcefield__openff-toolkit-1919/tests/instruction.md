# Issue

**Title:** Update some inherited method signatures flagged by mypy 1.11

## Description

- [x] Update some method signatures that don't agree with inherited base class
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1feb83d484b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.