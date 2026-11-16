# Issue

**Title:** fix(opt): minimise with the dispersion k-space settings MD uses

## Description

.

`perform_min` never read `kspace_dispersion`. With `kspace_dispersion: pme` or `ewald`, `so3lr opt` and the relaxation before MD therefore used real-space dispersion, while the MD that follows (and flash opt) used k-space dispersion.

`perform_min` now reads the four `kspace_dispersion*` settings as `perform_md` does, passes the method and interpolation nodes to `load_model`, and builds the dispersion grid with `setup_kspace_grid`.

Tests: `tests/test_min_kspace_dispersion.py` checks that `perform_min` passes `kspace_dispersion` and its interpolation nodes to the model, with and without `pme`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `217c7ed02345`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.