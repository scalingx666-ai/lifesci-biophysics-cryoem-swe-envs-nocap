# Issue

**Title:** Also raise error on empty InChi key

## Description

This is a brief follow-on to #1898 and should finish out #1897

IIUC, the key is just a hash of the InChi representation so it can be tested with the same setup. A counter-example would be a case in which either the InChi or its key can be empty, but not the other. Based on this, I didn't make a new error, but that could be done.

- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e34e786c2fc6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.