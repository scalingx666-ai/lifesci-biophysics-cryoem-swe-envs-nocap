# Issue

**Title:** Add automatic versioning and packaging

## Description

for details. This PR adds a github action that runs bump2version on pushes/merges to the master branch and follows it by publishing the package to PyPi. For now, I am using the test PyPi server.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed7aef3f404a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.