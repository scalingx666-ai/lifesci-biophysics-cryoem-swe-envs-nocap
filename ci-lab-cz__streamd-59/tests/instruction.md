# Issue

**Title:** Enable gmxMMPBSA GBSA and PBSA parsing

## Description

## Summary
- allow using run_gbsa parsing helpers without heavy MDAnalysis/RDKit dependencies by importing optional utilities lazily
- add regression test to ensure FINAL_RESULTS_MMPBSA.dat files yield separate GBSA and PBSA energy outputs

## Testing
- `pytest streamd/tests/results_parse_test.py -q`

------
https://chatgpt.com/codex/tasks/task_e_68a5fe961e44832b9a10a6e89f6cf82f

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bfe962ea689`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.