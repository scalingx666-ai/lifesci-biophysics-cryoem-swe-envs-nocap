# Issue

**Title:** ReplicaExchangeReporter supports atomSubset option

## Description

DCDReporter and XTCReporter both have an option for specifying which atoms to write.  This PR makes ReplicaExchangeReporter expose that option to reduce the size of replica exchange trajectories.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cc4ca4600f73`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.