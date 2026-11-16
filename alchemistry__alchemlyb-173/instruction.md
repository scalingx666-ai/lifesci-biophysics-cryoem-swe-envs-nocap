# Issue

**Title:** fixed AMBER parser logging

## Description

- log to module logger and not the root logger
- removed redundant ERROR and WARNING text
- removed redundant trailing newlines

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c70bb369ddaf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.