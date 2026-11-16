# Issue

**Title:** Replace numpy in1d usage with numpy.isin

## Description

## Summary
- replace deprecated `numpy.in1d` calls with `numpy.isin` to maintain compatibility with recent numpy versions
- update doctest examples accordingly

## Testing
- not run (not requested)

------
[Codex Task](https://chatgpt.com/codex/tasks/task_b_6953ee76aa7c8322bbf181faf8b6bef1)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `008674db258a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.