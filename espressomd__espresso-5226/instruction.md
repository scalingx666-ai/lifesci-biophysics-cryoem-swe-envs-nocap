# Issue

**Title:** Rename and document `Analysis.particle_energy()`

## Description

Description of changes:
- API change: rename `Analysis.particle_energy()` to `Analysis.particle_non_bonded_energy()` to better reflect the calculated quantity (kinetic, bonded, electrostatic and magnetostatic contributions are not part of this energy)
- bugfix: Thole corrections are no longer part of the energy calculated by `Analysis.particle_non_bonded_energy()`
- improve documentation of the per-particle non-bonded energy analysis function and of the Thole correction

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f74e2d57e4b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.