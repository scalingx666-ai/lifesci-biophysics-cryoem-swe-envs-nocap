# Issue

**Title:** Move the reduction into the output model

## Description

In order to predict per-atom properties (e.g. partial charges), the reduction operation has to be redefined. Currently, the reduction operations are limited to the ones supported by `scatter` (https://github.com/rusty1s/pytorch_scatter).

This PR make the reduction operation parts of the output model and allows the model-specific implementations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a555405942dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.