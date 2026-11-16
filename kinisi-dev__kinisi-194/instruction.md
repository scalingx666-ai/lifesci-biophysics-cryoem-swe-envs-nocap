# Issue

**Title:** Fix problem with dimensionality

## Description

As highlighted with #193, currently inputting a single dimension doesn't work. This PR resolves this issue and adds appropriate tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a84d12a4b45`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.