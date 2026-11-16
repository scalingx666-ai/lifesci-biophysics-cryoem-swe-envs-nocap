# Issue

**Title:** Make cc1/2 in cctbx.xfel.merge deterministic

## Description

This changes the cc1/2 code by changing how odd/even lattices are selected.  Instead of using the 'id' column, which is dependent on the number of mpi ranks being used, use a hash of the experiment identifier.  This gives the same results no matter how many ranks are used.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea37ef013586`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.