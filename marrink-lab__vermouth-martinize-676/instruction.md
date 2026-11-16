# Issue

**Title:** pause tests for dssp executable

## Description

With reference to #672 update the tests to run on `ubuntu-latest`, and accordingly remove support for the dssp executable.

This requires moving MDTraj from an optional dependency to a hard dependency.

Any code referring to a dssp executable has been removed. Possibly some test files can also now be removed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `666374674706`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.