# Issue

**Title:** Fix missing names in ExternalMDSnapshot

## Description

`ExternalMDSnapshot` was moved to a different file, and didn't get all its module-level names redefined. Bug uncovered by @jasperdenduijf in #907.

~~Right now, this still requires tests~~ (tests added!). We're missing coverage on the lines that caused that part of the issue in #907, so we should have tests covering those lines.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `647ceb05bbcc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.