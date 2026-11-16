# Issue

**Title:** Rename covalent radii constant

## Description

## Summary
- rename covalent radii dictionary to `COVALENT_RADII`
- deprecate `covalentradius` binding
- update exports and interaction utilities to use `COVALENT_RADII`
- drop unnecessary deprecation of `COVALENTRADIUS`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("PDB")'`

------
https://chatgpt.com/codex/tasks/task_b_68c03a7ddd20832db0f84077910749c3

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5cf0c046f1cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.