# Issue

**Title:** Mark standardize as deprecated

## Description

Following #277 and some internal discussion we are moving away from Dataset standardisation.
We believe its current implementation is not the best fit for NNPs but for the moment we are leaving the option there for backwards compatibility. It will be removed at some point.

We could instead simply remove the feature altogether or make it more sensible, like offering alternative approaches as to how the mean is computed.

This is your last chance if you have an opinion about this!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0a2106b19d4c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.