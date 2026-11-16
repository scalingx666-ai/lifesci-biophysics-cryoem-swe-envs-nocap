# Issue

**Title:** Stagger interpolation for core torsions that are turning on or off.

## Description

This PR addresses issues where a core torsion is turning from off to on with a dependency on a non-linear angle terms. For example, morphing a linear H-C#C-H into a H-O-O-H requires us to first partially convert the angles *before* we turn on the associated torsion. This PR also adds a system constraint to ensure that torsions (both propers and impropers) are never defined over linear angles. 

Note that dummy torsions that are being turned on/off are already interpolated at the 0.7 to 1.0 range.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `205b1f04ef7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.