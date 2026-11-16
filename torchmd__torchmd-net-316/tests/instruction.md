# Issue

**Title:** Filter non used sample values in QM9

## Description

Samples provided by the QM9 dataset included a bunch of unused tensors, like "edge_index".
This was slowing down training a bit by adding overhead when building batches.
This PR removes them via a pre_transform, leaving only "pos", "z" and "y".

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `669481686059`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.