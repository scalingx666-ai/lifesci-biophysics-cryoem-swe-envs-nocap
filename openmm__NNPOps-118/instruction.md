# Issue

**Title:** Use args when calling self.aev_computer

## Description

This PR allows the use of a forward hook on the internal AEVComputer of ANI models when using OptmizedTorchANI via TorchScript. See aiqm/torchani#649 for details.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d15cb9196e28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.