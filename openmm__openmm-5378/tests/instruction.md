# Issue

**Title:** Made writing checkpoints from ReplicaExchangeReporter optional

## Description

This adds a `checkpoints` argument to specify whether it should write checkpoints or not.

I also made it so all optional outputs default to false.  Previously `trajectoryPerState` defaulted to true for no good reason.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83e17ff26c9d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.