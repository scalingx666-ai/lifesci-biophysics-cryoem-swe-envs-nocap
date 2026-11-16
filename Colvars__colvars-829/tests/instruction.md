# Issue

**Title:** fix: try fixing the orientation sign

## Description

An attempt to fix #827

It look likes that the sign does not affect the dynamics in the NAMD tests, but I am not sure if the tests cover both the positive and the negative cases.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aa63575609b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.