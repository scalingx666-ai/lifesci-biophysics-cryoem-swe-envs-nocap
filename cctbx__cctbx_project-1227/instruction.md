# Issue

**Title:** iotbx.data_manager: build miller arrays once per file

## Description

## Why

Reading a reflection file through the DataManager cost three full
`as_miller_arrays()` builds, and the number grew with every DataManager
constructed in the process:

- `process_miller_array_file` runs `_filter_miller_array_child_datatypes`,
  which calls one filter per entry of `MillerArrayDataManager.miller_array_child_datatypes`.
  The parent filter (`filter_miller_array_arrays`) built the arrays, the
  map_coefficients filter built them again in `_child_filter_arrays`, and
  built them a third time for its complex-array scan.
- `miller_array_child_datatypes` is a class-level list, and
  `_add_miller_array_phil_str` appended to it on every `DataManager()`
  construction. The k-th DataManager in a process therefore looped over k
  copies of each datatype and built every file 3k times (measured on a
  small cif: 3, 6, 9 builds for three successive DataManagers).

`as_miller_arrays()` is the expensive step of a reflection read: seconds on
large deposited structure-factor cifs, so the DataManager path was 2.5x the
cost of a plain `any_reflection_file` read on both cif and mtz.

## What is done

- `_filter_miller_array_child_datatypes` builds the arrays once
  (`_build_miller_arrays`, honouring the `miller_array_skip_merge` option)
  and hands them to every filter through a new `miller_arrays=None`
  parameter. Each filter still builds on its own when called without it, so
  the public signatures keep working.
- `_add_miller_array_phil_str` appends a datatype to the class-level list
  only if it is not there yet.
- Regression test `test_miller_array_single_build` in
  `iotbx/regression/tst_data_manager.py`: writes a small mtz with an
  amplitude array and a map-coefficient pair, constructs four DataManagers in
  a row with different datatype sets and options, and asserts exactly one
  build per DataManager, the expected parent and child labels, and no
  duplicates in the class list. It fails on master with 3 builds.

## Gains

DataManager read in a fresh process (`DataManager().process_miller_array_file`
+ `get_miller_arrays`), deposited SF files, cctbx at 0fa242a223:

| file | before | after | plain any_reflection_file read |
|---|---|---|---|
| 4ybb-sf.cif (114 MB, 3.06 M reflections) | 12.07 s | 5.49 s | 4.74 s |
| 4ybb-sf.mtz | 7.86 s | 3.04 s | 2.65 s |
| 6i7v-sf.cif (88 MB, 1.26 M reflections) | 9.15 s | 4.19 s | 3.61 s |
| 6i7v-sf.mtz | 6.22 s | 2.43 s | 2.00 s |

The DataManager read is now within ~15% of a bare `any_reflection_file`
read on both formats; peak RSS of the reading process drops as well (4ybb
cif 2235 -> 1709 MB, mtz 1200 -> 805 MB). The arrays returned are identical
(checked against the direct build by the miller-array read benchmark's
parity gate).

## Notes

- The complex-array scan in `filter_map_coefficients_arrays` used to build
  with the default merge setting regardless of `miller_array_skip_merge`,
  while the other two builds honoured it. It now uses the same arrays as the
  rest, so the option applies uniformly. Merging only affects arrays with
  sigmas, which complex arrays do not have, so map coefficients are unchanged.
- Pre-existing and left alone: `get_map_coefficients_arrays()` with no labels
  raises "no known map_coefficients arrays" on master too, because the child's
  all-labels dictionary is never populated; the label-explicit call works.

## Testing

- `iotbx/regression/tst_data_manager.py` passes (including the new test).
- `libtbx.find_clutter` and `libtbx.find_unused_imports_crude` clean on the
  three files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35e155e021bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.