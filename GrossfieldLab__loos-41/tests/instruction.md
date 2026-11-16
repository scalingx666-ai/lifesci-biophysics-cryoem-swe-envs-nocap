# Issue

**Title:** smoothed sharper edges of cluster-kgs

## Description

Added squareness check in clusteringutils for matrix reader function. Added verbiage to fullhelp cautioning against using matricies that are not symmetric but are square.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe1b9033664b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.