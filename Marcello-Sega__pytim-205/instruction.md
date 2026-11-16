# Issue

**Title:** closes queues left open

## Description

Was able to reproduce under some conditions bug #114 
Closing open Queue() in `itim.py` seems to fix it

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b1e106954c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.