# Issue

**Title:** Resolve #1096 with an optional argument for <_generate_unique_atom_names()>

## Description

Continued from @PEFrankel 's original PR at   which was on a fork so some linting actions were sad and OpenEye CI wasn't running.

- [ ] Resolves: , with an optional suffix parameter. Avoids possible conflict with analyses that create atom dictionaries without referencing the resname. Also improves readability of simulation files.
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `142649644e00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.