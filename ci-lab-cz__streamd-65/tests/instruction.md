# Issue

**Title:** feat: allow replicate MD simulations

## Description

## Summary
- add a `--replicas` CLI option to run multiple simulations for each complex
- copy prepared systems and tweak seeds for replicate runs
- test replicate directory generation and track new feature in changelog

## Testing
- `ruff check streamd/run_md.py`
- `ruff check streamd/tests/md_test.py`
- `pytest`

------
https://chatgpt.com/codex/tasks/task_e_68af2e77043c832b87aa4b707ca2e5ac

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a5bcea6fddb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.