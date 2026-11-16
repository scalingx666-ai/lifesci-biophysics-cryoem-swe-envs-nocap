# Issue

**Title:** fixed Gromacs 5.x syntax for MDrunner

## Description

Although MDrunner is not configurable, it should work most of the
time with default setup (i.e., with the 'gmx' driver command).

For anything else, derive a new class.

 (can be reopened if demand is there)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69cb158d6987`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.