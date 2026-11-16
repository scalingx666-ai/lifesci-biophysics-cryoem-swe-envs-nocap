# Issue

**Title:** OpenMM RC testing: Switch ndays to 3

## Description

I got the expected failure with `ndays` set to 30, so this PR is just to reduce that down to 3.  for overall test design.

I'll merge this immediate on passing tests; all needed review was done in discussion of #1019.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9f875043f1fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.