# Issue

**Title:** Auto-enable per-residue decomposition from mmpbsa.in

## Description

## Summary
- Automatically turn on per-residue decomposition when `&decomp` block exists in `mmpbsa.in`
- Drop the `--decomp` command-line flag and rely on `mmpbsa.in` contents

------
https://chatgpt.com/codex/tasks/task_e_68a619060354832b8cd5c5ac628b83d1

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83fbf47138e8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.