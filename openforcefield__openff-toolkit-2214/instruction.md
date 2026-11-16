# Issue

**Title:** Update examples to use Sage 2.3.0, so make rdkit+nagl required and drop OE examples builds

## Description

- [x] Use Sage 2.3.0 in examples
- [x] Treat NAGL as required in examples
- [x] Treat RDKit as required in examples
- [x] Do not install AmberTools in examples
- [x] Do not install OpenMMForceFields in examples
- [x] Update other dependencies to minimum versions
- [x] Re-run notebooks
- [x] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f98d20a17873`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.