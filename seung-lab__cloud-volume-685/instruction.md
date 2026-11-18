# Issue

**Title:** perf: much faster mem:// speed

## Description

Avoid serialize/deserialization cycles.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `245a4bd2c249`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.