# Issue

**Title:** Fix: extend CommandClient.Error from Exception instead of BaseException

## Description

Otherwise errors thrown when connection to command servers are lost dont get caught by `except Exception`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f0a1fca965cf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.