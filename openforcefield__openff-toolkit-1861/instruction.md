# Issue

**Title:** Disallow SMIRKS-based lookup of virtual site parameters

## Description

This was kinda clunky to implement since elements in `VirtualSiteHandler._parameters` don't know about the handler they're ultimately stored inside of

Implements one idea of 

- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53db31fdb3ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.