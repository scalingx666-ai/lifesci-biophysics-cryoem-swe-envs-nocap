# Issue

**Title:** replaced useless function "any_none()" with a check for "None in"

## Description

This PR removes the useless function `any_none()` which does the same work as an `if None in`, and the relative test is removed. 

This was suggested in issue #236.

I'm not introducing a check for `mbar==None`, maintaining the logic as before.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffbf35429892`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.