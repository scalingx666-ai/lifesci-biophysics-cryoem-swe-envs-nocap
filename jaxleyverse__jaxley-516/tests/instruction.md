# Issue

**Title:** Deprecation handling

## Description

Added some code to streamline deprecating stuff. The basic idea: mark some function with `@deprecated("0.5.0")` and this will a) warn the user and b) trip tests if the version is bumped to 0.5.0 or higher.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32068e9e7d46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.