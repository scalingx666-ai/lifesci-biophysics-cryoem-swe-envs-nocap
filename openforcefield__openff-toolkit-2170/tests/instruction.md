# Issue

**Title:** Fix pytest config, warnings

## Description

- [x] Do not always run slow tests
- [x] Get working with `pytest` 9
- [x] Consolidate config to `pyproject.toml`
- [x] Clean up some warnings emitted in tests
- [x] Clean up some repeated warnings
- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f9a673ec5c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.