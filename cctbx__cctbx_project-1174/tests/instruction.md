# Issue

**Title:** Residue quality score: continuous triage metric, chiral types, severity tuning

## Description

## Summary
Redesign the per-residue **quality score** in `mmtbx/validation/utils.py` into a continuous triage-priority metric, and add a regression test for it.

Three commits:
1. **Redesign residue quality score as a continuous triage priority metric** — replaces the previous discrete scheme with a continuous priority used to rank residues by how much attention they need.
2. **Differentiate chiral outlier types** in the score.
3. **Tune the score** — non-linear clash severity and a higher twisted-peptide penalty.

## Test
Adds `mmtbx/validation/regression/tst_utils.py`, covering the severity helpers (`_clash_severity`, `_cbeta_severity`, `_bond_angle_severity`), the overall `calculate_overall_residue_quality_score`, and ranking invariants. Passes locally.

## Notes
This metric is consumed by MolProbity-side tooling; no other code in `cctbx_project` calls these functions (the change is additive within `utils.py`), so the blast radius is limited to the residue-quality scoring.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82c5215bce71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.