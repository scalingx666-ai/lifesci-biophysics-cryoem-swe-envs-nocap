# Issue

**Title:** Update born.f

## Description

Fixed an edge case bug for Gyrcuk Radii. 

If the radius (ri) of the atom being descreened is larger than sum of the separation distance (r) and scaled descreening radius (sk), then there is no descreening. 

This could happen for carbon atom (ri ~= 2) engulfing its bonded hydrogens (sk < 1).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `617988cc35c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.