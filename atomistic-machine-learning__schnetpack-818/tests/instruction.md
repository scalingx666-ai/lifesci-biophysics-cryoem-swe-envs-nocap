# Issue

**Title:** Fix stress normalization for left-handed cells

## Description

## Problem
Both `Forces` and `Response` divide stress by the signed cell volume. Swapping two lattice basis vectors leaves the physical lattice and model energy unchanged, but reverses the reported stress.

## Change
Normalize by the absolute physical volume in both response paths. The energy, force, API and checkpoint formats are unchanged. Existing docstrings clarify the volume convention.

## Verification
- A deterministic harmonic-pair regression checks both response APIs against independent analytic forces and stresses in a mixed-handedness batch with unequal cell volumes. Both new cases fail on the base and pass with this change.
- `python -m pytest tests/atomistic -q --tb=short`: **15 passed**, with four existing ASE/NumPy deprecation warnings.
- Actual PaiNN + Strain + AtomsConverter on periodic H2, with two lattice rows exchanged: energy remains unchanged; stress now agrees across handedness. Maximum error against all-nine-component central energy/strain differences improves from `9.86e-5` to `2.43e-14` in both response APIs.
- Changed-file Ruff lint and format checks pass. Independent read-only AI source/test review found no blocking issue.

Verified on Windows CPU, Python 3.12.10 / Torch 2.14.1. No GPU, full-repository, or scientific-accuracy benchmark claim. Stress-dependent results for left-handed cells intentionally change and should be reevaluated.

AI-assisted implementation and numerical validation. I supervised and checked the AI-assisted work as it progressed. This is not maintainer approval.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6f20ba81110e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.