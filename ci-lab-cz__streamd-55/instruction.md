# Issue

**Title:** Prevent list conversion for boolean flags in config parsing

## Description

## Summary
- ensure `parse_with_config` keeps `store_true`/`store_false` options as booleans
- add regression test for boolean `store_true` option loaded from YAML

## Testing
- `pytest -q`

------
https://chatgpt.com/codex/tasks/task_e_68a46945ab0c832b8542b23c8c5826da

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c079dd38e529`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.