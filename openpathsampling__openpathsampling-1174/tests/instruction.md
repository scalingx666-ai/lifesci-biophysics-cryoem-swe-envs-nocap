# Issue

**Title:** Drop numpy pin

## Description

We pinned numpy to <2 while numpy 2 was in RC mode on testpypi because I was trying to release OPS 1.6.1, and MDTraj wasn't ready for numpy 2 yet. It's been a while. Let's release that pin. I'm hoping this will help make it easier to clean up some of the notebook tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd8ebfc1054f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.