# Issue

**Title:** Fix DataFrame.sort_index future warning

## Description

The test suite gave the following future warning:
```python
openpathsampling/tests/test_lookup_function.py::TestVoxelLookupFunction::test_df_2d
  /home/sroet/github_files/openpathsampling/openpathsampling/numerics/lookup_function.py:268: FutureWarning: In a future version of pandas all arguments of DataFrame.sort_index will be keyword-only
    df = df.sort_index(0).sort_index(1)
```
066f87a implements the fix for that (make the arguments keyword based instead of location based).
90c75cf solves the `pep8` complaints my coding environment was noticing

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8b091c92916`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.