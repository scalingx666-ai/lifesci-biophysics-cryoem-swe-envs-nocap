# Issue

**Title:** Handle cases in which NetCDF file is not readable

## Description

While the simulation is running, it is sometimes temporarily impossible to read the NetCDF file, causing the `status` command to crash. This handles that case.

Before merging, I still want to test this works on the cluster. I'll have to wait for this case to crop up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffcb928fbb29`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.