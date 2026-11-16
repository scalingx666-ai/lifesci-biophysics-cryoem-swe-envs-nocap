# Issue

**Title:** Add function-first wrappers for non-mutating filters

## Description

## Summary
- support `filtersequences` with function-first syntax
- add matching `filtercolumns` method and tests

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("MSAEditing.jl"); MIToSTests.retest("MSAEditing.jl")'`

------
https://chatgpt.com/codex/tasks/task_b_68c40d73c0fc832dbd501b34f8a04b4b

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `defd2ec5a19f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.