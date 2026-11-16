# Issue

**Title:** CI on all branches

## Description

Forgot to include style-check and codeql pipelines in previous PR #276. Also makes the type-checking bot message clearer when there's no difference with target branch in terms of coverage

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1c5d419ba953`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.