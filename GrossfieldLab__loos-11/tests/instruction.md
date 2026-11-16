# Issue

**Title:** precision tool option for RMSD tools

## Description

This adds a command line flag to the four tools that produce RMSD matrices as output to write a user specified number of digits for each matrix element (as a tool option, using the program options framework) rather than two, the value that had previously been hard-coded into all four tools.

Currently the long flag is "--precision" and the short flag is "-p".

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8725d3338afb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.