# Issue

**Title:** Update NAGL pins and default model in tests

## Description

This just updates NAGL pins to allow use of the rc3 model. It also updates tests to the rc3 model, which seem to pass without any modification required.

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4ebc06f34c18`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.