# Issue

**Title:** Improved pdb_wc

## Description

Addresses some of the points raised in #12.

Added detailed summaries for pdb_wc when called with options. Dropped the gap reporter because `pdb_gap` does that and 'one tool, one job'.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e4cf5f90271`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.