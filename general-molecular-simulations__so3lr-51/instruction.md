# Issue

**Title:** fix(finetune): train periodic data with the long-range terms eval and MD use

## Description

Fine-tuning on periodic data computed its long-range terms differently from `so3lr eval` and MD.

**What was wrong**
- Every `model:` key in a finetune config was dropped, so periodic data trained with the pretrained `cutoff_lr` of 1000 Å cut off where the pair list ends, and k-space could not be turned on.
- The k-space mesh was sized from the first cell only.
- Structures that are not periodic in 3D got only the screened real-space Coulomb under k-space, with no error.
- Molecules and crystals in one batch broke: with a crystal first, the batch failed on the molecule's missing offsets; with a molecule first, every crystal silently lost its long-range image offsets.

**Changes**
- A finetune config may set `model.cutoff_lr`, `kspace_electrostatics`, `kspace_smearing`, `kspace_spacing` and `kspace_dispersion` (plus `kspace_dispersion_smearing` / `_spacing`). They apply to training only and are recorded under `long_range` in `fine_tuning.json`; the saved hyperparameters keep the checkpoint's values.
- When a finetune config turns on k-space, every setting it leaves unset takes the value `so3lr eval` and `so3lr md` use by default: smearing 4.0 Å, spacing 2.0 Å and no k-space dispersion. The model is then trained with the long-range terms it is run with. `so3lr train` is unchanged (k-space dispersion on, on the electrostatic grid), except that `model.kspace_dispersion: false` now turns it off.
- `train` and `finetune` refuse periodic data whose `data.neighbors_lr_cutoff` is below `model.cutoff_lr` (or when `cutoff_lr` is unset), and say to set both equal, e.g. 12 Å.
- k-space training sizes the mesh from the longest cell row per axis over the training and validation data, and refuses structures that are not periodic in 3D.
- When the data also holds periodic structures, non-periodic graphs get a zero cell and zero integer offsets, so mixed and crystal-only batches share one compiled step. `so3lr eval --save-to` still writes those molecules as non-periodic.

**Known limits (TFDS data)**
- The cutoff check and the mesh sizing see only the first TFDS batch; a gas-phase structure later in a k-space run stops it with an error.
- The TFDS loader still strips the zero cell from molecules, so molecule-only and mixed batches compile separately.

Tests: `tests/test_periodic_training.py` (mixed batches in both orders, mesh size incl. a triclinic cell, dispersion k-space off, gas-phase structures refused under k-space) and two cases in `tests/test_cli_finetune.py` (refusal; PME fine-tune with the eval/MD defaults recorded in `fine_tuning.json`). CI 10/10.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c26c6fb3cd4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.