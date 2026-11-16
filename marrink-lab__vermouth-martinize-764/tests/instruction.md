# Issue

**Title:** IDR cross interaction removal

## Description

In order to ensure that we removed Go potentials that crossed folded/IDR domain boundaries, the ComputeWaterBias processor was called twice in the executable, because that function was hidden there. This PR moves that functionality to its own processor, and extends the functionality to elastic networks, addressing #721.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c03da30aa58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.