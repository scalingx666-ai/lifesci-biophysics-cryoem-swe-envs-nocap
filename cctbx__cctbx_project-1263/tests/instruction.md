# Issue

**Title:** iotbx.pdb.hierarchy: add root.sym_equiv_residue_groups

## Description

## Problem
@russell-taylor asked for a symmetry selection that works like
`residues_within`: the symmetry copies of every residue with at least one
atom within a given distance of the model, to add to the model on new
chains. Nothing in cctbx gives that:
- `super_cell.get_siiu` returns single atoms, like `within`.
- The `residues_within` selection keyword selects only the model's own
  atoms.
- `super_cell.manager` copies the whole model under every operator, keeps
  residues on a 1 A grid, and returns a deep copy, so the original atom
  objects are not kept.

## Changes
- `iotbx.pdb.hierarchy.root.sym_equiv_residue_groups(crystal_symmetry,
  radius, selection=None, min_distance_sym_equiv=None)` returns, per
  operator, the operator, the `i_seqs` of the atoms copied, and their
  copies as a new hierarchy, with anisotropic U rotated and sigxyz and
  siguij cleared. The model is not changed. It sits beside
  `expand_to_p1` because it works on hierarchies, and builds on `get_siiu`.
- A residue with an atom on a symmetry element is copied under every
  operator that puts that atom in range, including the element's own.
- A copied atom on a same-element atom of the model or of an earlier copy
  is left out, so a residue on a symmetry element is not copied onto
  itself; one that straddles an element can come back partial.
- `super_cell.get_siiu(keep_site_symmetry_ops=True)` gives every operator
  that puts a site on a symmetry element in range, not one per
  equivalent, and `get_sym_equiv_tables` takes an existing
  `site_symmetry_table`, so the method builds it once. The defaults keep
  the current behaviour.
- The fixtures and brute-force oracles of
  `cctbx/crystal/tst_sym_equiv_near_sites.py` move to
  `cctbx/crystal/sym_equiv_test_utils.py`, shared with the new test.
- Not in this PR: appending the copies to a model. That takes a few lines
  with `iotbx.pdb.utils.chain_id_generator`; whether it belongs in
  `iotbx.pdb.hierarchy` or `mmtbx.model` depends on what the caller holds.

## Tests
- New `iotbx/regression/tst_hierarchy_sym_equiv_residue_groups.py`
  (4 s), registered in `iotbx/run_tests.py`. The copies match a
  brute-force search over all operators and lattice shifts in 18 space
  groups (the whole table with `--AllSpaceGroups`, 19 s), and in six
  special-position cases (two-fold, mirror, three-fold, inversion centre,
  m-3m). Also covered: a small example with its PDB output, rotated
  anisotropic U, atoms left out on the model or on an atom of the same
  element, the tolerance, bool and index selections, operator order and
  denominators, multi-model input.
- `cctbx/crystal/tst_sym_equiv_near_sites.py` checks
  `keep_site_symmetry_ops` against its brute-force search in the
  special-position cases, and rejects a site symmetry table built for
  other sites.
- macOS (Darwin 27), Python 3.11: `libtbx.run_tests_parallel module=cctbx
  module=iotbx module=mmtbx` ran 839 tests; 39 fail, none from this
  change: 34 pass on Linux with this branch, and the other 5 fail the
  same way at the merge base.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.304, macOS |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (link them,
      or say "none found"): builds on #1230, which replaced the closed
      #1223; no issue for this request
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes (none;
      one method added, and two optional arguments in `super_cell`)
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9fae373aec59`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.