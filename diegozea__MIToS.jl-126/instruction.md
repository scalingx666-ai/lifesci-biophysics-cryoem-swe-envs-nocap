# Issue

**Title:** Add Pfam PDB mapping branch tests

## Description

## Summary
- test duplicate PDB annotations in `getseq2pdb`
- verify `msacolumn2pdbresidue` with blank Pfam numbers, warnings vs strict mode, and mismatched PDB names using a canned SIFTS file
- ensure overloads without explicit SIFTS paths return the same mapping
- bundle all SIFTS test cases in `dummy_sifts_file.xml`

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("Pfam"); MIToSTests.retest("Pfam")'`

------
https://chatgpt.com/codex/tasks/task_b_685d41fd0f68832d8f5904d6ea15b19c

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0280abf3c3af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.