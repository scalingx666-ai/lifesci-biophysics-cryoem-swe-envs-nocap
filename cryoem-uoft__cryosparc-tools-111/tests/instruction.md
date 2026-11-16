# Issue

**Title:** fix(spm): correctly encode enums in query params and headers

## Description

If given an enum list of enums, it should encode their values.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c06473ded8db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.