# Issue

**Title:** Add percent positive score

## Description

## Summary
- add `_check_samelength` helper for pairwise sequence functions
- use the helper in identity and similarity routines
- add MSA method for `percentpositive` and corresponding tests
- make `matrix` a keyword for MSA percent positive and allow positional `out` argument
- refactor `percentpositive` to share length check logic with new `_percentpositive`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("Identity")'`

------
https://chatgpt.com/codex/tasks/task_b_686a20644b14832da82b84d5c5876e0e

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1aef5407245`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.