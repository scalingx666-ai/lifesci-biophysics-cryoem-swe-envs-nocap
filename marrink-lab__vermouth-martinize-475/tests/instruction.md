# Issue

**Title:** Update gh actions

## Description

The GH workflow started complaining about soon-to-be deprecated node.js versions, so I figured I'd update the actions we use. In addition, I took the liberty to update the python versions we test against. In particular, I dropped py3.6 and added py3.10

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe1b28fe6eb6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.