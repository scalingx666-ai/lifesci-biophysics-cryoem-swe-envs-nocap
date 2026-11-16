# Issue

**Title:** Cleanup and bugfixes for FAH generator for small molecule transformations

## Description

## Description

This PR contains numerous improvements and bugfixes for the FAH generator, focusing on small molecule transformations.
These updates will be tested in sprint-11A (which will not show up on the progress bar) and subsequent revisions that attempt to improve on sprint-11.

## Motivation and context

 and other issues with FAH project generation.

## How has this been tested?

Initial tests use `perses/tests/test_fah.py`.
This PR will not be merged until `sprint-11A` is successfully running and the dashboard is fully operational.

## Change log

Improvements for the Folding@home perses generator:
- [x] Cleaned up and documented `perses/app/fah_generator.py`
- [x] Reorganized file writing so that files are written as the data is available for easier debugging
- [x] Improved metadata output to better track useful provenance information, including initial/final SMILES and molecule titles
- [x] Added explicit list of solute atoms to be written to `xtcAtoms` in `core.xml`
- [x] Added sufficient information to map hybrid solute atoms to {old | new} solute atoms ()

## Notes for future

This code will need to be completely overhauled after we replace the top-level perses API with a streamlined class-based container hierarchy.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67b7a719dc38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.