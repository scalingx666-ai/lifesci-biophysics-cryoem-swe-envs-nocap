# Issue

**Title:** feat: use the app for API requests

## Description

Instead of making people forward baseport + 2, can make requests directly via the app at base port. Greatly simplifies setup when using tools with remote CryoSPARC instances.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6e0c972a145`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.