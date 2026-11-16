# Issue

**Title:** [WIP] Allow duplicate SMIRKS and have lookups return the LAST one found

## Description

- [x]  
    - If multiple parameters have identical SMIRKS, this will make `__getitem__` calls return the LAST instance in the ParameterList/ParameterHandler (rather than the first)
    - Adds named argument `ParameterHander.add_parameter(..., allow_duplicates=True)`
    - Since the default behavior remains the same, this won't affect amber-ff-porting https://github.com/openforcefield/amber-ff-porting/blob/c6d3412f5c94cd1c3ed1b8b6ba56909fe4706080/ConvertResidueParameters.py#L747
- [x] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c7a0ba1a2839`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.