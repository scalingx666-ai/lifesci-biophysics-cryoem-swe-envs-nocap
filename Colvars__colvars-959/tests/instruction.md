# Issue

**Title:** fix: reject the use of "pmf on" without "pmfColvars"

## Description

I don't know why people would use "pmf on" without "pmfColvars" or leaving it empty, but there are.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6299b82c2abb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.