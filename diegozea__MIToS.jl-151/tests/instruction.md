# Issue

**Title:** Implement change_b_factor utility

## Description

## Summary
- add `change_b_factor` to `PDBResidues` and export it
- remove unused `_change_B` helper
- use new function in documentation
- test `change_b_factor`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDBResidues")'`

------
https://chatgpt.com/codex/tasks/task_b_686681517354832da13b90f937f8929e

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `11e08eddae27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.