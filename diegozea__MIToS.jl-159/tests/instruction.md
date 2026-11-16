# Issue

**Title:** Run JuliaFormatter on unformatted files

## Description

## Summary
- Apply JuliaFormatter to src/MSA/Concatenation.jl, test/MSA/MultipleSequenceAlignment.jl, and test/PDB/Contacts.jl

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("MSA"); MIToSTests.retest("Concatenation"); MIToSTests.retest("Concatenation"); MIToSTests.retest("MSA"); MIToSTests.retest("MultipleSequenceAlignment"); MIToSTests.retest("MultipleSequenceAlignment"); MIToSTests.retest("PDB"); MIToSTests.retest("Contacts"); MIToSTests.retest("Contacts")'`

------
https://chatgpt.com/codex/tasks/task_b_68bfefe8c5f0832d9b50c4b6b9232b7c

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac37b61c4aa3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.