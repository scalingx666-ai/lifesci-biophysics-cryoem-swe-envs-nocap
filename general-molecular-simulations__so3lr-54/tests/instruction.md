# Issue

**Title:** feat(training): data-parallel training on all local GPUs (training.data_parallel)

## Description

Adds `training.data_parallel` (default `false`). When it is on, `fit_from_iterator` (the TFDS path) trains on all local devices:
- each optimizer step takes one padded batch per device;
- gradients and metrics are averaged across devices (`pmean` inside `shard_map`), and every replica applies the same update.

Behaviour:
- The effective batch is the device count times one batch. `eval_every_num_steps` still counts optimizer steps.
- Parameters, optimizer state and EMA are replicated on every device. Validation and checkpoints use a single-device copy, so checkpoints keep the single-device format.
- The checkpoint records the device count, and resuming on a different count is refused, because the effective batch would change. Checkpoints written before this have no count and resume as before.
- A partial set of batches at the end of an epoch (fewer than the device count) is dropped. An epoch with fewer batches than devices raises an error instead of finishing without a step.
- The two-stage loss and reduce-LR-on-plateau both work.
- The non-TFDS `fit()` path logs a warning and stays single-device.

Tests: `tests/mlff_tests/test_data_parallel.py`, on 4 simulated CPU devices. The step equals one update on the averaged gradient (Adam and Muon), and `fit_from_iterator` matches a single-device run on the averaged batches, through checkpointing, a restart and EMA. The same test checks that a resume on another device count, and an epoch with fewer batches than devices, are both refused.

Tested on 4 A100 GPUs, with this branch merged into current main, using the XL-S model on two QCell sets:
- 4-GPU training: 0.28 s per optimizer step of 4 batches, against 0.23 s per batch on one GPU, so 3.2x the throughput. Validation loss at equal steps matches the one-GPU run (2.48 vs 2.51 at step 192).
- Resume through `so3lr train`: it continues from the step-192 checkpoint without a jump in validation loss (2.48, then 2.45 at step 208).
- Resume of the same checkpoint on one GPU: refused with the device-count error.
- Fine-tuning `so3lr-tauri-s-2606` with the long-range anchors on 4 GPUs: runs, and the anchor metrics are logged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70707f5bb0c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.