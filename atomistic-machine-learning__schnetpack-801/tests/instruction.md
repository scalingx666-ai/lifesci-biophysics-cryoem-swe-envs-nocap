# Issue

**Title:** Jl/atomwise fix

## Description

The avg aggregation mode in the atomwise layer did not work for multiple outputs. 
Now, after swapping the averaging and squeezing, it works.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ee82f62b8f1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.