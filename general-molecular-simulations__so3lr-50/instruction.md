# Issue

**Title:** feat(finetune): three recipes that keep the long-range heads intact on energy/force-only data

## Description

Fine-tuning on energies and forces alone corrupts the charge, a0 and C6 heads, because that data cannot separate short-range from long-range energy. This PR adds three recipes that prevent this, and documents them in `so3lr/config/finetune.yaml`.

## Recipes

1. **Anchors (best accuracy).** `training.long_range_anchor_weights` anchors the charges, a0, C6 and, optionally, dipoles to a frozen copy of the starting model on every batch. The loss targets are `anchor_<quantity>`. It costs one extra forward pass per batch, which made runs 2–6% longer.
2. **Exact long range.** The new `--strategy readout_only` freezes the backbone and the long-range heads. A new residual energy readout (`model.energy_residual_mlp_widths` / `energy_residual_inputs`) is zero at initialisation and can be set from a finetune config. Charges, a0/C6 and dipoles stay bit-identical to the pretrained model.
3. **Pseudo-labels.** This is recipe 1 run offline: the teacher's predictions are written into the data and trained as targets. It needs the new per-atom `partial_charges` target, read from an extxyz array and NaN when absent.

The comment on `freeze_long_range` now says what it actually does: the frozen heads read a backbone that keeps training, so the charges and ratios still drift.

## Evidence

**Solvated Protein Fragments [1] (revPBE-D3), `so3lr-tauri-m-2606`, 100k training frames, energy weight 1.0:**

| Arm | E (meV/atom) | F (meV/Å) | Δq rms (e) | ΔC6 | PLA15 | QM7b angle |
|---|---|---|---|---|---|---|
| Base | 9.0 | 230 | 0 | 0 | 3.8 | 3.3° |
| `full` | 4.9 | 42.7 | 0.035 | 62% | 10.3 | 11.5° |
| `freeze_long_range` (weight 0.01) | 9.7 | 42.5 | 0.033 | 14% | 8.1 | 13.4° |
| **1. anchors** | **4.1** | **42.2** | 0.006 | 0.8% | 6.0 | 6.0° |
| **2. `readout_only` + residual** | 5.8 | 43.7 | 0 | 0 | 5.0 | 3.3° |
| **3. pseudo-labels** (weight 0.01) | 18.6* | 42.1 | 0.006 | 0.8% | 4.7 | 6.1° |

\* At energy weight 0.01 the energies do not train; after the offset refit the error is 8.8 meV/atom. Recipe 3 was tested only at weight 0.01 and matched anchors at the same weight to the third digit.

**Molecular subsets of MAD-1.6 [2] (r2SCAN, ShiftML crystals + fragments), `so3lr-tauri-s/m/l-2606`, periodic, 12 Å + PME, energy weight 1.0.** Each cell is S / M / L:

| Arm | E (meV/atom) | F (meV/Å) | Δq rms (e) | ΔC6 | PLA15 | QM7b angle |
|---|---|---|---|---|---|---|
| Base | 17.3 / 16.1 / 16.2 | 129 / 118 / 111 | 0 | 0 | 3.7 / 3.8 / 6.4 | 4.1° / 3.3° / 2.9° |
| `full` | 7.2 / 7.7 / 6.7 | 68 / 55 / 44 | 0.026 / 0.015 / 0.020 | 15 / 24 / 14% | 35.0 / 33.6 / 36.5 | 13.6° / 9.5° / 11.8° |
| **1. anchors** | 8.1 / 7.5 / **5.2** | **68 / 55 / 43** | 0.008 / 0.007 / 0.005 | 1.1 / 1.0 / 0.7% | 32.2 / 30.3 / 35.1 | 10.1° / 5.9° / 7.0° |
| **2. `readout_only` + residual** | 9.0 / 8.7 / 6.2 | 75 / 63 / 52 | 0 | 0 | 27.2 / 25.3 / 29.9 | 4.1° / 3.3° / 2.9° |

- **The ranking holds at every size.** Anchors match `full` on forces while keeping a0/C6 within 1.1%; `full` drifts C6 by 14–24%.
- **`readout_only`** keeps the heads exact and reaches 87–88% of the anchors' force gain (99% on Solvated Protein Fragments).
- **Every recipe underbinds** non-covalent complexes on these labels (PLA15 25–36 kcal/mol). MAD's r2SCAN has no dispersion correction, and SO3LR keeps its own dispersion term. The guidance in `finetune.yaml` now says that labels must treat dispersion as SO3LR does.

PLA15 [3]: interaction-energy MAE (kcal/mol). QM7b angle: mean angle between predicted and LR-CCSD reference dipoles [4]. Δq rms, ΔC6: drift of the partial charges and C6 ratios from the starting model on the test set.

## Data
1. O. T. Unke, M. Meuwly, *Solvated protein fragments dataset*, Zenodo (2019), [10.5281/zenodo.2605372](https://doi.org/10.5281/zenodo.2605372); paper: *PhysNet*, [arXiv:1902.08408](https://arxiv.org/abs/1902.08408). Random 100k / 5k / 10k split, seed 0.
2. C. Malosso et al., *High-quality, high-information datasets for universal atomistic machine learning*, Materials Cloud Archive, [10.24435/materialscloud:jc-9f](https://doi.org/10.24435/materialscloud:jc-9f) (files `mad-1.6-r2scan-{train,val,test}.xyz`); paper: [arXiv:2603.02089](https://arxiv.org/abs/2603.02089).
3. J. Kříž, J. Řezáč, *J. Chem. Inf. Model.* **60**, 1453 (2020), [10.1021/acs.jcim.9b01171](https://doi.org/10.1021/acs.jcim.9b01171).
4. Y. Yang et al., *Sci. Data* **6**, 152 (2019), [10.1038/s41597-019-0157-8](https://doi.org/10.1038/s41597-019-0157-8).

## Review follow-ups
- One alias path for the a0/hirshfeld ratio targets in the loss; the parameter merge uses flax's tree helpers; the teacher shares the output masking of the energy/force function.
- `so3lr train` builds the model with partial charges when they are a loss target.
- Re-fine-tuning a residual checkpoint saved without `energy_residual_inputs` compares against its default `'last'`.
- A warning when the strategy freezes the residual readout (it would stay zero).
- The teacher keeps no second parameter copy, and its labels stay on the device.

## Not in this PR
- Periodic fine-tuning fixes (follow-up):
  - `cutoff_lr` / k-space settings for fine-tuning;
  - a PME mesh sized over all cells;
  - batches that mix molecules and crystals.
- The docs-site page (`training/finetuning`).

## Tests
New: `tests/mlff_tests/test_finetune_long_range.py`, plus cases in `tests/test_cli_finetune.py` and `tests/test_finetuning_strategies.py`. CI 10/10.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `70707f5bb0c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.