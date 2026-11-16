# Issue

**Title:** Improve atom clash detection

## Description

## Summary
- rename `is_peptide_bond` to `peptide_bond`
- rework `vanderwaalsclash` API and add index-based helper
- update covalent warning message
- test the new clash helpers

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDB")'`

------
https://chatgpt.com/codex/tasks/task_b_6874385763ac832dafa39a7eef0a40fb

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3249a89cc39`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.