# Issue

**Title:** Use more threads for CI

## Description

The CI scripts specify two threads/processes for compiling and running tests.  At some point, Github updated their [configurations](https://docs.github.com/en/actions/reference/runners/github-hosted-runners#standard-github-hosted-runners-for-public-repositories) so most free runners have four cores instead of two.  This updates the scripts accordingly.  Hopefully it should make builds run faster.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e8643105201`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.