# Issue

**Title:** SimStore: Type identification

## Description

SimStore storable functions (CVs) no longer need to have result type provided. Instead, is it identified when the results are first saved. This changes some of the SimStore API, which is only relevant to anyone already using SimStore-based CVs (which are not only experimental, but not even in a release yet.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67518ed69602`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.