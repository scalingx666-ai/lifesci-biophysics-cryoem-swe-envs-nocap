# Issue

**Title:** Remove deepsource

## Description

@FanwangM @PaulWAyers I'm proposing to remove deepsource and sourcery AI as they are distract more than they help in PR reviews. With copilot reviews being fairly robust these days, these older solutions are becoming redundant.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa4d09dfe62a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.