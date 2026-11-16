# Issue

**Title:** Attempt at better check for EM map

## Description

Hope this is a better way of deciding if we have an em map.
Remove what I believe to be redundant check if(space_group_number == 0) as it is set to 1 earlier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb81d54ec13b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.