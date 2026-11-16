# Issue

**Title:** Handle zero-length percent identity comparisons

## Description

## Summary
- avoid dividing by zero in `_percentidentity` by returning `NaN` when every column is ignored
- ensure the Float64 unit tests explicitly check NaN results for all-gap and all-XAA inputs
- prefer `isnan` for NaN assertions in the percent-identity Float64 tests

## Testing
- julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("Identity"); MIToSTests.retest("Identity")'

------
https://chatgpt.com/codex/tasks/task_b_68d55c0b7b4c832db0852ad1a9e83115

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10bd9689ff5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.