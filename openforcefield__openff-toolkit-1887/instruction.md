# Issue

**Title:** Add option to skip valence check when assigning vsites

## Description

Some FF researchers reached out to say they'd like to experiment with assigning vsites in different contexts than we intended to support. This PR adds the ability to set the `OPENFF_UNSAFE_VSITES` env variable to `"1"` to circumvent this check.

- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fdf383261717`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.