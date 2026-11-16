# Issue

**Title:** Add `Topology.clear_positions`

## Description

This adds `Topology.clear_positions()` which does what the name implies, instead of allowing `Topology.set_positions(None)` as I originally advocated for in #1820. Today I think it's better to just have a separate method that does this one thing - it handles what I remember my use case to be - instead of allowing `set_positions` to take different types.

I hold this preference weakly (I've already changed my mine once on my own) and can easily be talked out of it


- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `118918c1d7a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.