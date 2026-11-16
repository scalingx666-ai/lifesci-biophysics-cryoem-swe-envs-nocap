# Issue

**Title:** Implementation of temperature REMD

## Description

Includes:

- Types `ReplicaWrapper` and `ReplicaSystem` for handling replica exchange simulations. `ReplicaSystem` is equivalent to `System` but containing multiple replicas at different temperatures. `ReplicaWrapper` is a wrapper for a particular replica.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7cf63d607e1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.