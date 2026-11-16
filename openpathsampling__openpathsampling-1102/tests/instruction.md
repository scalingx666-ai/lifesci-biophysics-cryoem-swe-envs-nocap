# Issue

**Title:** Add FutureWarning for default filesetter external engines

## Description

this adds a `FutureWarning` to let users know that in OPS 2.0 the default `FileSetter` will become random string

This was part of the discussion on #1101

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd2cad582c15`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.