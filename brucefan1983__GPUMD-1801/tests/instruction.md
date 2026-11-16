# Issue

**Title:** Check nep.in against nep.txt for every model type

## Description

## Summary

`Parameters::check_existing_model` in `src/main_nep/parameters.cu` compares the model hyperparameters in `nep.in` with the header of `nep.txt` when `nep.txt` is an input, in prediction mode or when resuming from a `nep.restart`, and compares the row count of `nep.restart` with the number of parameters `nep.in` implies when resuming. Both checks ran only for `model_type 0`, so a dipole, polarizability or temperature model was read with whatever shape `nep.in` implied. A dipole model predicted with `basis_size 10 8` against `10 10` in its `nep.txt` ran to completion with exit code 0. The same model resumed from its `nep.restart` alone read the first 1581 of 1637 rows and also ran to completion.

This PR applies both checks to every model type. A mismatched `nep.in` now stops with the input error that a potential model gives.

## Testing

`tests_pytest/test_nep_model_consistency.py` gains four tests. `test_prediction_of_a_tensor_model_with_matching_nep_in_runs` predicts with the dipole model `tnep-water-polarization.txt` and the polarizability model `tnep-BaZrO3-susceptibility.txt` and a `nep.in` that matches each header, and requires a clean run. `test_prediction_of_a_tensor_model_with_mismatched_nep_txt_is_fatal` predicts with the dipole model and `basis_size 10 8` and requires the input error. `test_resume_of_a_tensor_model_with_matching_nep_restart_runs` and `test_resume_of_a_tensor_model_with_mismatched_nep_restart_is_fatal` resume both tensor models from a `nep.restart` without its `nep.txt`, the latter with `basis_size 10 8`. The two fatal tests fail on master, where `nep` exits with 0.

- `pytest test_nep_model_consistency.py test_nep_seed.py` in `tests_pytest`: 38 passed
- `git-clang-format` on `src/main_nep/parameters.cu`: no changes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c77ce2fb720`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.