# Issue

**Title:** Fix AnnotatedSequence slicing

## Description

## Summary
- handle `AbstractRange` indices for `AnnotatedSequence` by deferring to array behavior
- keep annotation updates when selecting arbitrary column vectors
- confirm AnnotatedSequence column slicing via updated tests
- check annotation columns and residues after indexing

## Testing
- `julia --project -e 'push!(LOAD_PATH,"test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("getindex")'`

------
https://chatgpt.com/codex/tasks/task_b_6865a6f5c380832da01789a4ef95ee95

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `11e08eddae27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.