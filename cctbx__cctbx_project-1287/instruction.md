# Issue

**Title:** iotbx.pdb: Recognise ELECTRON MICROSCOPY and ELECTRON CRYSTALLOGRAPHY in EXPDTA

## Description

## Problem
ELECTRON MICROSCOPY and ELECTRON CRYSTALLOGRAPHY, two of the wwPDB
[EXPDTA](https://www.wwpdb.org/documentation/file-format-content/format33/sect2.html#EXPDTA)
techniques, are missing from the EXPDTA keyword list, so
`get_experiment_type()` is empty for PDB-format cryo-EM and electron
crystallography models, and `is_electron_microscopy()` holds only for mmCIF.
`experiment_type` has no test for electron diffraction.

## Changes
- `iotbx/pdb/records.py`: ELECTRON MICROSCOPY and ELECTRON CRYSTALLOGRAPHY
  join the EXPDTA keywords.
- `iotbx/pdb/experiment_type.py`: `is_electron_diffraction()`, true for
  ELECTRON CRYSTALLOGRAPHY or the legacy ELECTRON DIFFRACTION.

## Tests
- `iotbx/pdb/tst_pdb.py` (`exercise_records`) covers the three EXPDTA terms;
  it fails without the fix and passes on macOS arm64 and Linux.
- Full Phenix suite on Linux (t96): no new failures compared to master.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.311 |

## Checklist
- [x] Searched open and closed PRs and issues for prior work: #1285 (its
      review asks for `is_electron_diffraction()`)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a7892cf3c42`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.