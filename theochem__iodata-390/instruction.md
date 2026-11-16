# Issue

**Title:** Update for load_one info in gro files

## Description

This PR is a small update for gro files without velocities data. Also, I added a dictionary in the periodic.py file for atomic mass and element symbol relation (atmass2sym).

## Summary by Sourcery

Handle GRO files that omit velocity columns and expose atom names in GROMACS loader metadata while adding element–mass lookup tables.

New Features:
- Introduce element atomic-mass-to-symbol and symbol-to-atomic-mass mapping dictionaries in the periodic data module.

Bug Fixes:
- Allow loading GRO trajectory frames that do not include velocity columns by treating velocities as optional and propagating None when absent.
- Expose atom names rather than atom types in the force-field parameter metadata returned by the GROMACS loader.

Tests:
- Add regression test and sample GRO file without velocities to verify correct handling of missing velocity data and metadata.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08593d3283f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.