# Issue

**Title:** cctbx.crystal.super_cell: omit identity contacts between general-position sites

## Description

## Problem

`get_sym_equiv_tables()` builds a full `pair_asu_table` within the radius and
extracts it with every operator in both directions. Almost all of it is
contacts inside one asymmetric unit: identity contacts between two general
sites, which `get_siiu()` skips (`if (rt_mx_ji.is_unit_mx()): continue`). In
reduce2 this table sets the run's peak memory. The table for the symmetry copies
(`sym_equiv_residue_groups(cs, 3.6)` over every atom, H included) is the peak
with copies on, the default. With `add_h_to_water=True`, the water step's
table (`_symmetry_environment()`, 3.86 Å over every atom) is the peak on 1r27
(4wsc has no waters, so its water step builds no table).

| model, table | pairs | identity contacts | held after build | high-water mark while building |
|---|---|---|---|---|
| 4wsc, copies (109,494 atoms) | 1,984,066 | 1,982,074 (99.90%) | 244–254 MB | +292 to +345 MB |
| 1r27, copies (55,483 atoms) | 1,025,450 | 1,021,640 (99.63%) | 127–129 MB | +87 to +123 MB |
| 1r27, water step | 1,243,070 | 1,237,304 (99.54%) | 154–156 MB | +97 to +114 MB |
| 6vw1, water step | 544,914 | 543,272 (99.70%) | 68 MB | +66 MB |

## Changes

- `get_sym_equiv_tables()` builds the same special-position settings, site
  symmetry table and asu mappings (buffer `radius * (1 + 1e-6)`). It takes the
  pairs from `crystal.neighbors_fast_pair_generator` with `add_all_pairs()`'s
  cutoff, `minimal=True` and `min_cubicle_edge=5`, and calls
  `pair_asu_table.add_pair(pair)` only for the pairs that
  `asu_mappings.is_simple_interaction(pair)` rejects. That C++ test is false
  when either site is on a special position; otherwise it is true when the
  two asu mappings have the same operator and unit shifts, that is, when
  `rt_mx_ji` is the identity. The table is extracted as before. The argument
  checks are unchanged.
- **What the table no longer holds:** identity contacts between two
  general-position sites. Sites on special positions keep their identity
  contacts. The table still has a row for every site.
- **Why `get_siiu()` is unchanged:**
  - Its main loop skips every identity operator.
  - For a site on a special position, `add_pair()` records the site-symmetry
    equivalents of each of its contacts. Those are non-identity operators that
    `get_siiu()` uses.
  - With `keep_site_symmetry_ops`, `get_siiu()` reads the identity contacts in
    a special site's own row to decide whether the site is in range of the
    selection.
  - Every pair with a special site is therefore kept, identity included.
- **Other readers:** `get_siiu()` is the only reader of `pair_sym_table` from
  these tables. Its callers are:
  - `sym_equiv_residue_groups()`;
  - `water_protonation._sym_equiv_pairs()`;
  - `super_cell.manager`;
  - qrefine `check_qm_sees_crystal_neighbours()`;
  - the tests.
- **Interface:** `pair_sym_table` of the returned `sym_equiv_tables` holds
  fewer interactions than before. The docstrings of `get_sym_equiv_tables()` and
  `sym_equiv_tables` say so. No signature changes.

## Tests

New, in `cctbx/crystal/tst_sym_equiv_near_sites.py` (already registered):
- `exercise_siiu_as_from_every_contact(space_group_info)`, run in the
  space-group loop:
  - It compares `get_siiu()` on the new table with `get_siiu()` on a table
    holding every contact (built with `add_all_pairs()` as before).
  - It compares the whole return: the dict and its order, each operator
    list in order, and the list of distinct operators in order.
  - Cases: 8 general sites, one site on each Wyckoff special position, four
    sites moved by lattice translations; radii 2.5 and 5 Å; five selections;
    both `keep_site_symmetry_ops`. It checks that the special sites are the
    ones the structure was built with.
  - Leaving the special sites' identity contacts out of the table makes it
    fail in 10 of the 18 default space groups and in 157 of 231 with
    `--AllSpaceGroups`. Leaving out every pair with a special site makes it
    fail in 15 of 18 and in 218 of 231.
- `exercise_identity_contacts_only_leave_the_table_empty()`: three sites in
  contact only within one asymmetric unit give a 3-row table with nothing in
  it, and `get_siiu()` returns `({}, [])`. It fails on master, and fails
  again with the filter removed.

The `is_simple_interaction()` filter gives the same tables as the first
version of this pull request (which tested `rt_mx_ji` and the special flags in
Python): all 462 tables of the test's structure (231 space-group settings,
radii 2.5 and 5.0 Å), every operator in the same order.

Run on macOS 26.6.1 (arm64), Python 3.11, all pass on this version:
- `tst_sym_equiv_near_sites.py`: default (11.5 s), `I41/acd` and
  `--AllSpaceGroups`.
- `tst_hierarchy_sym_equiv_residue_groups.py`, `tst_super_cell.py`,
  `tst_water_protonation.py`, `tst_reduce2_symmetry_copies.py`,
  `tst_reduce2_water_h.py`.

Run on the first version (same tables), all pass: the mmtbx reduce2 test
files (53) and qrefine `tests/unit/tst_13, 15, 34, 38, 46, 62`.

reduce2 models from the first version are byte-identical to master in nine
output cases and in twelve paired memory comparisons; descriptions match
after masking run stamps, directories and timings. Output cases:
- `add` on 4wsc, 1r27 and 8afw;
- 6vw1 and 6KK8 with flips;
- `add_h_to_water=True` on 4wsc, 1r27 and 6vw1;
- one `optimize` run.

This version gives models byte-identical to the first version's on `add` 4wsc
and 1r27 and on `add_h_to_water=True` 1r27.

## Performance

Peak memory in reduce2 (`time -l`, MB, mean of 3 runs in the same batch;
master against the first version, whose tables this version reproduces):

| run | master | this pull request |
|---|---|---|
| `add` 4wsc | 1530 | 1268 |
| `add` 1r27 | 880 | 800 |
| `add_h_to_water=True` 4wsc (no waters) | 1537 | 1276 |
| `add_h_to_water=True` 1r27 | 994 | 790 |

One run each of this version: 1226, 768 and 771 MB (`add` 4wsc, `add` 1r27,
`add_h_to_water=True` 1r27).

The table is now built faster than on master. `get_sym_equiv_tables()`, best
of 3 in fresh processes, on reduce2's sites (H included):

| model, radius | master | first version | this version |
|---|---|---|---|
| 4wsc (109,494 sites), 3.6 Å | 1.75 s | 2.77 s | 1.02 s |
| 1r27 (55,481 sites), 3.6 Å | 0.91 s | 1.49 s | 0.57 s |
| 1r27, 3.86 Å (the water step's radius) | 1.10 s | 1.78 s | 0.67 s |

In the reduce2 runs, the water step on 1r27 took 4.48 s with the first version
and 3.34 s with this one (one run each).

## Coding agent

Written with Claude Code in a local checkout, directed step by step by the
author.

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness and version | Claude Code 2.1.322, macOS |

## Checklist
- [x] Searched open and closed PRs and issues for prior work (link them,
      or say "none found"). Builds on #1223, #1230 and #1263 (all closed);
      no open PR or issue touches `super_cell.py` (searched 2026-11-09).
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c9d1d90e634b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.