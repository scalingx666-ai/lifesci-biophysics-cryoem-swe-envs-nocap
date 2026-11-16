# Issue

**Title:** Raise error when encountering empty InChi string

## Description

This is the surface-level fix for #1897 

- [ ] Raise same error from `to_inchikey`?
- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24b2d7f97c9b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.