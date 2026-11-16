# Issue

**Title:** Guard gcc_version comparisons

## Description

get_gcc_version() returns None when the compiler is not gcc or version detection fails. Therefore check for None before doing numerical comparisons. This was already done in many places but this PR catches a few more.

Fixes cctbx/cctbx_project#1127

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `761ff65f32d7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.