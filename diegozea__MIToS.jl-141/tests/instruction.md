# Issue

**Title:** Optimize MMCIF dictionary creation

## Description

## Summary
- preallocate MMCIF dictionary arrays in `_pdbresidues_to_mmcifdict`
- write directly into vectors without `push!`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDB")'`

------
https://chatgpt.com/codex/tasks/task_b_686394433f14832da09668d591a902c3

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `565356be6ae3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.