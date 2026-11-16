# Issue

**Title:** Performance enhancement for `md.join`

## Description

This PR aims to resolve potential performance issues for `md.join`, especially when joining a large amount of pieces together.

The details of the problem is described in Issue #1961. Here's the revised code and benchmark. It is obvious the new version is orders of magnitude faster than the old code.

![image](https://github.com/user-attachments/assets/9cd648e0-b86f-420a-a516-641d2d613e02)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2e0f5a4dcc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.