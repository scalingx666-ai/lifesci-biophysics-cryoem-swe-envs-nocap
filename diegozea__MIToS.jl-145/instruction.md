# Issue

**Title:** Implement rename_columns! for MSAs

## Description

## Summary
- support renaming of MSA columns
- export `rename_columns!` and `rename_columns`
- test column renaming functionality
- update rename_columns! to modify annotations (HCat) when present

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("MultipleSequenceAlignment")'`

------
https://chatgpt.com/codex/tasks/task_b_686407cffe7c832daf35c1a03ed93356

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c113881dc6b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.