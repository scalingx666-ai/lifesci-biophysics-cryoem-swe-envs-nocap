# Issue

**Title:** Bug/fix undefined variables

## Description

I found that there were undefined variables. If they could be resolved immediately, I resolved them; otherwise, I added a `FIXME` comment. 
I also removed import lines if a module was not accessed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29a5b1c33da6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.