# Issue

**Title:** fix(tune-ewald): take the model's electrostatic and dispersion scales

## Description

`so3lr tune-ewald` estimated the real-space errors with `electrostatic_energy_scale = 4.0` and `dispersion_energy_scale = 1.2` written into the code (`TODO load from model_config`). The Tauri models have a dispersion scale of 1.0, so their dispersion cutoff errors were estimated for another damping width.

`model_energy_scales(model_path)` reads both scales from the model's `hyperparameters.json`, and `tune()` passes them to the error estimates.

Tests: `test_error_estimates_take_the_models_scales` in `tests/test_tune_ewald_error_bounds.py`, for each bundled model and the default.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `869761bb09d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.