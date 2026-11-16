# Issue

**Title:** Fix #652

## Description

raise helpful error if dssp executable is not found. Can easily happen if `-dssp` is given with no path (i.e. user expects to use MDTraj dssp functionality) but MDTraj is not installed in the python environment. Will fix #652

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f86d35d8c46d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.