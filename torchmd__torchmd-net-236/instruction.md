# Issue

**Title:** Fix Tensornet accuracy bug

## Description

In CUDA graph mode in tensornet static shapes require processing invalid neighbor pairs (which are passed down as (-1,-1) pairs).
By blaming all extra neighbor pairs to the first particle the overall accuracy could be slightly affected in some situations.
I fix this by instead blaming the extra pairs to a ghost atom instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f837503b9dae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.