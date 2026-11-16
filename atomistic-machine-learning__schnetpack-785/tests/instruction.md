# Issue

**Title:** Fix EMA callback: resume training from the raw weights, not the EMA a…

## Description

Checkpoints are written during validation, when on_validation_epoch_start has copy_to()'d the EMA average into the model; restore() only runs at the next on_train_epoch_start. A run resumed from such a checkpoint therefore loads the EMA average as its model weights, and on_fit_start immediately called store(), overwriting the stored raw weights — so the subsequent restore() was a no-op and training silently continued from the EMA average instead of the raw weights a continuous run would have used.

Two changes:
- on_fit_start: after loading the EMA state on resume, restore the raw weights (the state's collected_params, captured by the store() at on_validation_epoch_start before the save) into the model.
- on_validation_epoch_start: skip store()/copy_to() during the sanity check. on_fit_start has already stored the raw weights and copied the EMA average in, so the sanity check still validates the EMA weights; storing again would overwrite the raw weights with the average and reintroduce the bug whenever num_sanity_val_steps > 0 (the default).

Fresh (non-resume) fits are unaffected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de850ebf5dc0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.