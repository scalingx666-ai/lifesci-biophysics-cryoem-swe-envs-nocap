# Issue

**Title:** Critical bugfixes for SAMS release

## Description

This fixes bugs in two of the features I added to the SAMS release:
* Cray detection now correctly checks environment variables instead of static list of possible MPI environment variables (whoops!)
* FIRE minimization debug message now correctly prints `max_iterations` instead of nonexistent variable

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3d3812d11fc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.