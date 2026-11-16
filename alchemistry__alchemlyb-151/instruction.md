# Issue

**Title:** fix #149

## Description

fix #149
Sorry when I bump the pandas version to 1.2, I removed a line of codes that passes the attrs which shouldn't be removed (as new dataframe is being generated).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51c28a580f77`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.