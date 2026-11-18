# Issue

**Title:** perf(spatial_index): threaded, batched, and robust insert to mysql

## Description

Improves run time of inserts to mysql. Took 3h35m to upload 193M rows with these improvements. 

Note: Must use SSD not HDD for CloudSQL. 

sqlite will also benefit from these improvements.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81ee6a8ab31f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.