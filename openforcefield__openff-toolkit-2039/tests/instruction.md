# Issue

**Title:** Catch MissingOptionalDependencyError when nglview is not installed

## Description

.  Test `test_molecule.py::TestMoleculeVisualization::test_ipython_repr_no_nglview` already present and was failing (now passes, although this wouldn't have been caught by CI before; see ).

- [x] Tag issue being addressed
- [ ] ~~Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)~~
- [ ] ~~Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable~~
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4cc5706446ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.