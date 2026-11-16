# Issue

**Title:** So3lrCalculator: a structure that goes from free space to a box gets a periodic neighbour list

## Description

## So3lrCalculator: a structure that goes from free space to a box gets a periodic neighbour list

When one `So3lrCalculator` with the default glp neighbour list first evaluates a structure in free space and later the same structure in a box, the periodic call reused the neighbour list allocated without a cell, and returned the free-space energy and forces.

On water_64 (so3lr-tauri-s-2602, `lr_cutoff=6`, float64):

| Order of calls | Periodic energy (eV) |
|---|---|
| fresh calculator | −718.062635 |
| free space first, then periodic | −605.384312 (the free-space value) |

Periodic first, then free space, already gave the right free-space energy.

### Fix

The calculator now records whether glp's list was allocated with a cell (`_glp_periodic`). It updates the list only when that matches the current call, and allocates a new list otherwise.

### Tests

`tests/test_engine.py::test_one_engine_goes_between_free_space_and_a_box` now takes its free-space and periodic references from one calculator, free space first, so it fails without this fix (the periodic reference comes back as the free-space energy). Before, it used a fresh calculator for each to work around this bug. `test_ase.py`, `test_ase_small_cell.py` and `test_engine.py` pass on CPU.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59198f64d9e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.