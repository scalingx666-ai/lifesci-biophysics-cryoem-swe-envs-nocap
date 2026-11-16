# Issue

**Title:** Optimization to GPU implementation of Ewald

## Description

The GPU platforms were doing Ewald in an unnecessarily slow way.  This makes it about twice as fast.  It's still slower than PME for anything except tiny systems, but perhaps this makes it slightly more useful than before.  I suspect there might also be room for further optimization, if we choose to spend time on it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6e8643105201`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.