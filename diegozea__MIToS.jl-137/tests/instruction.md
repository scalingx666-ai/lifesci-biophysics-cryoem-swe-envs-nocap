# Issue

**Title:** Optimize TRP plane computation

## Description

## Summary
- add constant sets `_TRP_PLANE1_ATOMS` and `_TRP_PLANE2_ATOMS`
- refine `_get_plane` to use list comprehensions with these sets

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("PDB"); MIToSTests.retest("Piccolo"); MIToSTests.retest("Piccolo")'`

------
https://chatgpt.com/codex/tasks/task_b_685e9c280bd0832da2bc8846e6999a6b

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `98bf350da9aa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.