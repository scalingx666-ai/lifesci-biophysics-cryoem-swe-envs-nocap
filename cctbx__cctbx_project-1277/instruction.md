# Issue

**Title:** iotbx.pdb.hierarchy: element_is_positive_ion recognizes Ga

## Description

## Problem
`atom.element_is_positive_ion()` and `atom.element_is_ion()` return False
for Ga: the positive-ion list in `iotbx/pdb/hierarchy.cpp` has `"FA"`
where `"GA"` belongs.

## Changes
`"GA"` replaces `"FA"`, which is not an element symbol.

## Tests
- `iotbx/pdb/tst_hierarchy.py`: the positive-ion spot check includes
  `GA`; it failed before the fix with
  `AssertionError: GA was not seen as a positive ion`.
- macOS 27, arm64: iotbx suite, no new failures. mmtbx suite: not run.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.304, run locally on macOS |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (link them,
      or say "none found"): none found
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `31f70daa0528`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.