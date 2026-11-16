# Issue

**Title:** fix(training): save the final TFDS-training steps past the last eval

## Description

## Problem

`fit_from_iterator` (the TFDS training path) only validated and saved a checkpoint when `step % eval_every_num_steps == 0`. Optimizer steps taken after the last multiple were never saved. A resume restores the latest checkpoint and starts at the next epoch, so those steps were lost for good.

Example: on 4 GPUs, a 3-epoch run took 204 optimizer steps with `eval_every_num_steps: 16`. Its last checkpoint was step 192. Resuming to 4 epochs restored step 192 at epoch 3, so steps 193-204 were trained and then thrown away. At production settings (evaluating every ~1000 steps), a finished run's final checkpoint could be up to 999 steps out of date. This bug predates #54 and happens on a single device too.

`fit` (the in-memory path) already handled this case: after the loop it validates and saves the final weights through `_log_and_checkpoint`.

## Change

- The in-loop validation and the in-loop checkpoint save in `fit_from_iterator` are now two local helpers, `_evaluate` and `_save_checkpoint`. The loop calls them as before. After the loop they run once more when the call trained past the last multiple of `eval_every_num_steps`, the same condition `fit` uses. The final checkpoint therefore holds the same full training state as the periodic ones: epoch, batches consumed, EMA, reduce-LR, stage two and device count.
- Early stopping only fires on a multiple of `eval_every_num_steps`, so the final save always closes a finished epoch. A resume from it starts at the next epoch, as before.
- The extra evaluation does not update early stopping or reduce-LR. With that rule, a resume that adds epochs has the same control state as an uninterrupted run, which only evaluates on multiples.
- `_evaluate` reuses `_validation_metrics`, the helper `fit` already uses. Before, this function had its own copy of the same averaging code. The results are identical: every validation batch returns the same metric keys.
- The eval logging and the finite-loss save are shared with `fit`: `_log_and_checkpoint` is split into `_log_eval_metrics` and `_save_if_finite`, and `fit` keeps a two-line wrapper. The TFDS path's "Skipping checkpoint" notice now goes to the logger, as `fit`'s does.
- Inference loads the newest kept checkpoint. A finished run whose final checkpoint is kept therefore deploys the final-step weights instead of the last periodic ones.

## Tests

- `test_fit.py::test_fit_from_iterator_saves_the_final_weights` (new, sits next to `test_fit_saves_the_final_weights`): trains the real SO3krates model through `fit_from_iterator` for 1 epoch of 8 steps with an evaluation every 5 steps. It checks for checkpoints at `[5, 8]`, and restores step 8's training state through the real resume path (`_restore_tfds_training_state`): epoch 0, 8 batches consumed, 15 graphs.
- `test_tfds_pipeline_logic.py::test_fit_from_iterator_restart_matches_uninterrupted_updates` now also runs with `eval_every=3`. There, the first run's 2 steps reach no multiple, so the restart resumes from the final save. Params, EMA and optimizer state must match an uninterrupted run bit for bit.

Runs:
- CPU, `pytest tests/mlff_tests/` on this branch: **203 passed**, including `test_data_parallel.py` (also re-run after the refactor commit).
- CPU control (main's `training_utils.py` with the new tests): both new iterator cases fail. The checkpoints are `[5]` instead of `[5, 8]`, and the resume trains epochs `[0, 1]` instead of `[1]`. The `eval_every=1` case and the `fit` test still pass.
- 4×A100, `data_parallel: true`, the scenario from the top of this description: training ends with `ckpt_176, ckpt_192, ckpt_204`. `so3lr train` on the 4-epoch config prints "Restored TFDS training state ... at step 204" and "Re-start training from step 204 at epoch 3", and finishes at `ckpt_272`.

## Not changed

Orbax prunes with `best_fn`/`best_mode='min'` and `max_to_keep`. In orbax 0.12.4, `BestN` keeps only the N checkpoints with the lowest loss, so the newest checkpoint is pruned whenever its loss is not among them. Periodic and final saves are both affected. `latest_step()`, and therefore a resume, can then land on an older checkpoint. This PR leaves that behaviour as it was; #69 addresses it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5bcc2319b765`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.