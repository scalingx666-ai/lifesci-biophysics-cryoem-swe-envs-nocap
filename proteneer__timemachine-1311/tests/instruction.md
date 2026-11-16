# Issue

**Title:** Avoid reading in an invalid sample, triggering an invalid memory access

## Description

* Doesn't impact correctness of water sampling.
* Happens when the number of proposals left is fewer than the batch size. In this case the samples that are selected for the idx in the batch are stale and may be sampling invalid states. In the case that the final mol (by idx) was selected and it was a targeted inner move then the index of the new sample would be out of bounds triggering a failure. A very rare occurrence which is why this was not caught initially.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5bd58bb1e9ce`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.