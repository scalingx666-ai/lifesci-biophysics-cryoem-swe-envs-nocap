# Issue

**Title:** mmtbx.reduce2: Rename exclude_water to add_h_to_water, version 3.10.0

## Description

## Problem
reduce2 adds H to water with `exclude_water=False`, a negation.

## Changes
- `add_h_to_water` (default False) replaces `exclude_water`;
  `add_h_to_water=True` does what `exclude_water=False` did.
- Interface change: `exclude_water` is no longer accepted. In this
  repository only the reduce2 tests passed it; they use the new name.
- Version 3.10.0.

## Tests
- The tests that set the flag (`tst_reduce2_neutron_*`,
  `reduce2_neutron_utils.py`, `model/tst_exchangeable_hd_selections.py`)
  use `add_h_to_water=True`.
- Full Phenix suite on Linux: no new failures on the branch compared to
  master.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.311 |

## Checklist
- [x] Searched open and closed PRs and issues for prior work: none for
      this rename; the flag's history is #1150
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `063a76a37e8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.