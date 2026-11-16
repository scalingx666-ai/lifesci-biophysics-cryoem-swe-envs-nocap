# Issue

**Title:** Add tests for Pfam SIFTS helpers

## Description

## Summary
- add regression tests covering SIFTS file reuse and mapping cache behaviour in getseq2pdb
- exercise error handling when Pfam alignments lack an AC annotation and when SIFTS ranges do not overlap

## Testing
- `julia --project -e 'push!(LOAD_PATH, "test"); using MIToSTests; MIToSTests.retest("Pfam"); MIToSTests.retest("Pfam")'`

------
[Codex Task](https://chatgpt.com/codex/tasks/task_b_69107edacce4832db2397f8e284c0643)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1122bf982e37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.