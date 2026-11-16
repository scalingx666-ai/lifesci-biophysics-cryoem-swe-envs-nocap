# Issue

**Title:** Ensure CLI options override YAML defaults

## Description

## Summary
- Prevent YAML config values from overriding command-line arguments in run_md.py, run_gbsa.py, and prolif/run_prolif.py by filtering out CLI-specified keys before setting defaults



------
https://chatgpt.com/codex/tasks/task_e_68a217d08238832b8625727e2dbf9e5b

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09d8f8f7c2c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.