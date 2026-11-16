# Issue

**Title:** Drop use of `importlib_metadata`

## Description

`importlib-metadata` is a third-party package which backports features of `importlib.metadata` (which is in the standard library) to older Python versions. We were using it as a bridge while it was "provisional" but that ship has long passed.


This change is likely 3.10+ based on my reading of the docs, but I haven't pinned it down completely: https://docs.python.org/3/library/importlib.metadata.html

This was *not* declared as a dependency in test environments - it probably should have been - but is a requirement in packaging. This change would allow that to be dropped from the conda recipe.

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `713cfa90174b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.