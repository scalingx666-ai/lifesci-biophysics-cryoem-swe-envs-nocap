# Issue

**Title:** Fix MPI buffer-size mismatch in multibody combineAllWeightedSums (MPI_ERR_TRUNCATE)

## Description

Found the fix for my issue #1319  

In `MlOptimiserMpi::calculateExpectedAngularErrors()`, MPI rank 1 ("first_follower") calculates the per class/body rotation and translation accuracies. It syncs the best (minimum numerical value) of both vectors (stored in the scalar `acc_trans` and `acc_rot`) to the other MPI processes, who then each run the logic in `updateAngularSampling()` themselves. But as they're all operating from synced data they should reach the same conclusions, and most decisions are done based on these single "best" values so everything is fine.

Except that the decision in `updateAngularSampling()` whether or not to hold a particular body fixed needs the individual per-body values, thus the full vector. As it stands, this is not broadcast/synced and so the fixed-ness of bodies can diverge on other ranks because they never change their copy of this vector from the default initialization of all zeros. When bodies are being fixed on rank 1, this can lead to different `wsum_model.pdf_direction[]` sizes versus the other ranks and the `MPI_ERR_TRUNCATE` crash I describe in that issue.

This adds Bcast of the full per-body/class accuracies alongside the existing best/minimum value Bcast.

(A almost-entirely-pointless side effect of this is that, even for normal Refine3D runs, `run_it*_half2_model.star` files will now have real values instead of zeros in the printed `data_model_classes->rlnAccuracyRotations` & `rlnAccuracyTranslationsAngst` fields, whereas currently those are all zeros)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `210f68c865b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.