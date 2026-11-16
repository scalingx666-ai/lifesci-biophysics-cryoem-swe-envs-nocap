# Issue

**Title:** add per_type_coeffs option for weighted per-type force loss

## Description

## Description
Adds an optional `per_type_coeffs` key on `MetricsManager` metric entries and a `per_type_forces_coeffs` convenience kwarg on `EnergyForceLoss` and `EnergyForceStressLoss`. When supplied, the forces loss becomes the weighted mean of per-species MSEs: `sum(c_i * mse_i) / sum(c_i)`. Equal coefficients reproduce the current `per_type=true` equal-mean behavior exactly.

The low-level `per_type_coeffs` key generically handles weighting for any node field, while `per_type_forces_coeffs` acts as a convenience wrapper on standard loss classes (`EnergyForceLoss` and `EnergyForceStressLoss`) to unambiguously target the forces metric.

## Motivation and Context
Heterogeneous systems often contain one or two species that dominate the underlying physics. For ion conductors, force accuracy on the mobile ions is more important for resolving transport dynamics than on the largely immobile ions.

This change adds a per-type coefficient vector that weights the aggregation. The interface is purely additive: existing configs (with or without `per_type=true`) behave identically. Equal coefficients reproduce the current `per_type=true` mean exactly.

## How Has This Been Tested?
**Unit tests added** (3 functions, 8 instances total):
- `test_per_type_coeffs` — math correctness: equal coefficients reproduce the existing `per_type=true` mean exactly; non-equal coefficients produce `sum(c_i·mse_i) / sum(c_i)`.
- `test_per_type_coeffs_validation` — 6 parametrized error cases (missing `per_type`, unknown key, missing key, zero, negative, non-dict).
- `test_EnergyForceLoss_per_type_forces_coeffs` — wrapper routes the kwarg onto the forces metric only.

**Runs (Python 3.11, torch 2.12, lightning 2.6, CPU):**
- `pytest tests/unit/train/test_metrics_manager.py` — 460 passed (452 baseline + 8 new).
- `pytest tests/unit/` — 6349 passed; 32 errors in `test_ase_compile[...aotinductor...]` are pre-existing Apple Silicon MPS / float64 failures, unrelated (also fail on `main`).
- `pre-commit` — clean.
- end-to-end `nequip-train` run on A100-mig GPU.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
- [x] I have read ["Contributing to NequIP"](../docs/dev/contributing.md).
- [x] My code follows the code style of this project and has been formatted using `black`.
- [x] All new and existing tests passed, including on GPU (if relevant).
- [x] I have added tests that cover my changes (if relevant).
- [x] I have updated `CHANGELOG.md`.
- [x] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0cb886607c20`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.