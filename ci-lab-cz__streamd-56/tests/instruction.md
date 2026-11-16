# Issue

**Title:** Add gmx_MMPBSA analysis utilities

## Description

## Summary
- add optional residue decomposition and per-frame analysis to `run_gbsa`
- collect frame and decomposition CSV files into combined tables
- expose `--decomp` flag on GBSA CLI
- fix config parsing for boolean flags
- parse `gmx_MMPBSA` residue decomposition outputs and aggregate across complexes

## Testing
- `pytest`

------
https://chatgpt.com/codex/tasks/task_e_68a3870e7ee0832b869d1424aa2f3589

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d5800df189c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.