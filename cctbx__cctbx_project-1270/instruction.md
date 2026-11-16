# Issue

**Title:** iotbx.pdb.hierarchy, mmtbx.model: rotate anisotropic U as in #1263

## Description

## Problem
`sym_equiv_residue_groups` (#1263) rotates anisotropic U in its copies, but
`expand_to_p1`, `apply_rotation_translation` and the MTRIX/BIOMT expansion in
`mmtbx.model` did not rotate U or sigxyz; their docstrings noted that ANISOU
in the copies would be invalid. This is a  that gives all
four the same behaviour.

## Changes
- sigxyz and siguij are cleared under a rotation: they rotate only with their
  covariances, which the atoms do not hold.
- `atoms.rotate_uij_and_clear_sigmas` is in C++ so that copying stays
  vectorized.
- Behaviour changes: rotated copies lose sigxyz and siguij, and
  lattice-translation copies from `sym_equiv_residue_groups` keep them, since
  a translation leaves them valid.

## Tests
- `exercise_rotate_uij_and_clear_sigmas`, `exercise_expand_to_p1` and
  `exercise_apply_rotation_translation` (`iotbx/pdb/tst_hierarchy.py`),
  `exercise_mtrix` and `exercise_biomt`
  (`mmtbx/regression/model/tst_model_biomt_mtrix.py`), and
  `exercise_match_brute_force`
  (`iotbx/regression/tst_hierarchy_sym_equiv_residue_groups.py`).
- `exercise_expand_to_p1`, `exercise_apply_rotation_translation`,
  `exercise_mtrix` and `exercise_biomt` fail at the merge-base 6b3a9b6fe7.
- `iotbx` and `mmtbx` suites on macOS arm64: no new failures.
- Full Phenix suite on Linux: 15 failures, none caused by this change.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.304, run locally on macOS 27 (arm64) |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (none found;
      follows #1263)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1d7b005974c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.