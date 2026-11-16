# Issue

**Title:** Add back "True-True" CI

## Description

while attempting to be minimally invasive

This overlaps somewhat with #2121 and whichever is merged first should be accounted for in the other
 
- [x] Add back "OpenEye = True" and "RDKit/AmberTools = True" elements of CI matrix
- [x] Fix some minor typing issues
- [x] Fix/update some docstrings
---

- [x] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97ba56992536`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.