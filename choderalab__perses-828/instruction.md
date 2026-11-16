# Issue

**Title:** Fix RepartitionedHybridTopologyFactory tests to reduce stochastic failures

## Description

I’ve added a fix to the RepartitionedHybridTopologyFactory tests to reduce stochastic failures — we now check the potential energy of the proposed positions before generating the `RepartitonedHybridTopologyFactory`. If the potential energy is greater than 1e8, we try to generate another proposal with energy < 1e8. We do this 5x, and if on the 5th time we still fail to generate a proposal with energy < 1e8, we raise an exception.

Addresses:

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `02df62f7fc7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.