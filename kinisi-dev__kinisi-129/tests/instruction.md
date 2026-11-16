# Issue

**Title:** Triclinic unwrapping

## Description

Change calculate_displacements to handle triclinic cells correctly. This is done by comparing all 8 periodic images of the displacement vector, and taking the minimum magnitude one to be the true vector. This is a modification on the TOR unwrapping previously implemented (now in old_calculate_displacements).  This comes with a small change in time complexity and a large change in memory complexity. The memory usage seems to lower than other processes in `kinisi` but this needs testing. Also this change should be tested on model system to ensure it produces the desired unwrapping.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c40c92ba9fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.