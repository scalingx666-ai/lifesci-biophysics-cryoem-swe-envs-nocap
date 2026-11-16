# Issue

**Title:** docs: mention van der Waals radii change

## Description

## Summary
- note switch from Bickerton to Alvarez van der Waals radii in `vanderwaalsclash`

## Testing
- `julia --project -e 'using Pkg; Pkg.test()'` *(fails: Documenter precompilation error)*

------
https://chatgpt.com/codex/tasks/task_b_68c01b203cac832dbb0086f11c2f1bbc

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f8250253610f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.