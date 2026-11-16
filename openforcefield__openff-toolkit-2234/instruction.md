# Issue

**Title:** Fix some Pydantic V2 deprecations in tests

## Description

I couldn't find any other usages such as `.json()`. When I run tests locally, no Pydantic-related deprecation warnings are thrown

This forces the toolkit to use the V2 package at installation time, but that's already required to use Interchange 0.4.0 (released November 2024) or newer

- [x] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2d05cc7482e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.