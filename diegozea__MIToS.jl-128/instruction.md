# Issue

**Title:** Add sequence indexing tests

## Description

## Summary
- test AnnotatedAlignedSequence and AlignedSequence column selection
- fix getindex for AlignedSequence to keep matrix shape

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("getindex"); MIToSTests.retest("getindex")'`

------
https://chatgpt.com/codex/tasks/task_b_685e6c3ade68832d948c54ee9ae4645e

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `817cb6efc455`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.