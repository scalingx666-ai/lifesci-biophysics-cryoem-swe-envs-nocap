# Issue

**Title:** Use file name comparisons when finding force fields to load

## Description

When loading force fields, a criteria for matching is "does any file in relevant paths end with the requested force field name?" (More correctly, the _full, absolute path_ and including both the local directory and entry point locations.) This resulted in weird behavior @jaclark5 found, paraphrasing her case as "my local file is named `blah-opc3.offxml` and is returned when I try to load OPC3 with `ForceField("opc3.offxml")`, even though ... it's not"


- [x] Tag issue being addressed
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1e74cb7b927`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.