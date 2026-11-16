# Issue

**Title:** Fix doc build failure

## Description

Docs failed due to duplicate description of `reference_graphs` attribute: once in the class docstring, and once in the property docstring.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `019fb3130e46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.