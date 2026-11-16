# Issue

**Title:** XFEL small molecule improvements

## Description

New development work from January 2026 mail-in beamtime.

- Enable reflection subsampling for small_cell indexing
- Add two methods for distance refinement from spotfinder powder data. This is to enable automatic (and more accurate) calibration from a LaB6 standard. The simple (and effective) way is to add a `d_target` setting to `cctbx.xfel.powder_from_spots`, triggering a 1d grid search for detector distance corrections. A second experimental way is `cctbx.xfel.powder_refine_geometry`, which performs a least-squares refinement of detector displacements and tilts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `836498ee7f3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.