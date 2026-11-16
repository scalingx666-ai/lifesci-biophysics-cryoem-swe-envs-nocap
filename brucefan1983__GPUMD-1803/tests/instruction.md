# Issue

**Title:** Write the train outputs of a training run in the order of train.xyz

## Description

## Summary

`read_structures` in `src/main_nep/structure.cu` reorders the training structures before it cuts them into batches whenever `batch` is smaller than the number of structures. With the energy-difference loss, `EnergyDifference::read_train` places them in batches of its own. On master, `Fitness::predict` writes `energy_train.out`, `force_train.out`, `virial_train.out`, `stress_train.out`, `charge_train.out`, `bec_train.out`, `dipole_train.out` and `polarizability_train.out` batch by batch, and their rows follow the batch order. A reader that pairs a row with the frame of `train.xyz` at the same position, as calorine and most scripts do, attaches the predictions of one structure to another. With the four BaTiO3 structures of `tests_pytest/fixtures/training/train.xyz` and `batch 2`, the rows of `energy_train.out` come in the order 1, 3, 2, 0.

This PR writes every `*_train.out` file in the order of `train.xyz`, in training as in prediction, for potential, charge, dipole and polarizability models. `Structure` records its position in the xyz file as `index_in_file`. `predict` evaluates every batch, sorts the structures by that position, and writes the rows in the order of `train.xyz`. The training and test outputs share one writer, `write_predictions`, which writes the structures in the order given. The test outputs keep the order of `test.xyz` and are unchanged.

`test_batches_follow_the_group_order` in `tests_pytest/test_ediff_loss.py` read the batch order from the rows of `energy_train.out`. It now reads the batch membership from the energy RMSE that `loss.out` reports for each batch. The structures share one geometry, which makes that RMSE the spread of the reference energies per atom in the batch.

## Testing

- `tests_pytest/test_nep_train_outputs.py` trains a small potential for 1000 generations with `batch 1` and `batch 2`, and requires `energy_train.out` and `force_train.out` to follow `train.xyz`. Both cases fail on `d7c4a3e0`.
- Compared against `d7c4a3e0` for potential, qNEP with and without BEC, `charge_vdw`, `vdw`, `model_type 3`, and dipole and polarizability models with and without `atomic_v`, in training and in prediction mode, at several batch sizes. The train rows of `d7c4a3e0`, put back in file order, match. The test files are byte-identical. Force and BEC rows differ by at most 1e-5, which is the run-to-run noise.
- The rewritten energy-difference test fails when `ediff.in` links another pair.
- `pytest test_ediff_loss.py test_nep_train_outputs.py test_nep_model_consistency.py test_nep_seed.py`: 125 passed
- `git clang-format`: no changes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93d063b5e6e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.