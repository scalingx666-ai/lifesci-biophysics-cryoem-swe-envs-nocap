# Issue

**Title:** mmtbx.development.naiad: H-bond-aware placement of water hydrogens

## Description

## Problem
`reduce_hydrogen.place_hydrogens` (#1150) and
`ready_set_utils.add_water_hydrogen_atoms_simple` give every water the same
orientation, and `ordered_solvent.find_water_hydrogens` needs map peaks.
Nothing in cctbx orients water H toward their H-bond partners from the model
alone.

## Changes
- `mmtbx/hydrogens/water_protonation.py`: `place_water_hydrogens` points
  each water's two H at nearby H-bond acceptors, clear of the rest of the
  model and of other waters' H and away from metal cations, then relaxes
  the remaining clashes between waters. It honours crystal symmetry
  (special positions included) and altloc conformers.
- `mmtbx/programs/water_protonation.py`, `mmtbx/command_line/naiad.py`:
  the `mmtbx.development.naiad` program. The O-H length (X-ray or neutron)
  follows the model's experiment type.
- Supersedes #1175.

## Tests
- `mmtbx/regression/tst_water_protonation.py` (31 tests, about 5 s)
  passes on macOS arm64 and Linux.
- Full Phenix suite on Linux (t96): 6 failures, none new compared with
  master.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.304 |

## Checklist
- [x] Searched open and closed PRs and issues for prior work: #1175
      (earlier version of this PR, closed), #1150 (reduce2's identically
      oriented water H)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f67ac9fb139e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.