# Issue

**Title:** Fix off-by-one misalignment of permutations and frames in HREX output

## Description

Currently, `replica_idx_by_state_by_iter[i]` gives the permutation of replicas corresponding to the i+1st frame. This leads to confusion and potential bugs in analyses that try to correlate permutations and frames, e.g. to extract replica trajectories. (This also means that we confusingly store a final permutation that wasn't actually sampled by MD).

This PR fixes the off-by-one misalignment so that `replica_idx_by_state_by_iter[i]` gives the permutation of replicas corresponding to `frames[i]`.

Note, this does not affect the correctness of free energy calculations, or significantly affect existing diagnostic plots in timemachine.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ae3daf4ca91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.