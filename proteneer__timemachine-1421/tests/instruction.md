# Issue

**Title:** Enables minimizing system with only parts of system restrained

## Description

* Adds support for specifying a subset of the local idxs as being restrained in `minimizer.local_minimize`.
* Passes the ligand interacting atoms as the restrained atoms rather than restraining all of the free indices. Originally had tried restraining heavy atoms in  but code is a bit janky and didn't resolve any additional failures. Where hydrogen atoms clash and trigger failures, line search causes the ligand to blow up even if the hydrogens are unrestrained. @maxentile is working on a minimization approach that will resolve this in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `374a2de64596`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.