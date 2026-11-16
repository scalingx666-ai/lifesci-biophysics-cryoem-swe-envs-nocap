# Issue

**Title:** MP-Triage: score planarity outliers and the Arg chi5 twist

## Description

Adds planarity outliers and conjugation twists to the residue quality score in
`mmtbx/validation/utils.py`:

- Planes flagged in sigma use the bond/angle curve. Planes flagged for an atom
  tilted out of its ring use the same shape in tilt / limit.
- An Arg guanidinium twist about NE-CZ (chi5) past 20 deg scores on sin^2 of
  the twist, 1.0 at 20 deg to 10.0 at 90 deg, below a perpendicular peptide.
  Sugar N-acetyl and Asn-glycan link twists use the same curve.

The new inputs are optional keys, so scores from callers that don't supply them
are unchanged. Tests are in `tst_utils.py`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de2c3e5e7574`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.