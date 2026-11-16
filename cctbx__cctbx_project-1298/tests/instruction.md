# Issue

**Title:** Speed up the map-symmetry search (find_map_symmetry 8.6 -> 1.2 s, same results)

## Description

## Problem

`map_manager.find_map_symmetry` spent almost all of its time on Python overhead. On the 30x40x32 map that `map_model_manager().generate_map(d_min=3)` makes, one search took 8.6 s. For each of about 50 candidate symmetries:

- `ncs_group.is_point_group_symmetry` compared every product of two operators with every operator, one `is_same_transform` call at a time.
- `score_ncs_in_map_point_group_symmetry` moved and looked up one map site at a time.

Every caller pays this: `phenix.map_symmetry`, box tiling (`get_unique_set_for_boxes`), and local sharpening through `split_up_map_and_model_by_boxes`. `iotbx/regression/tst_map_model_manager_2.py` spent all of its 49 s in this search.

## Changes

- `mmtbx.ncs`: `is_point_group_symmetry` now compares a block of products with all target operators at once, using flex arrays.
  - A block is about 10000 product-target pairs, so memory stays bounded for large helical operator sets.
  - The arithmetic and tolerance tests are the same as in `is_same_transform`, in the same order. As in `is_same_transform`, the relative translation test is evaluated only where the absolute one fails.
- `cctbx.maptbx`: new `values_at_closest_grid_points(map_data, sites_frac)`, a vectorized `value_at_closest_grid_point` with the same rounding, wrapping and tie-breaking. `score_ncs_in_map_point_group_symmetry` uses it, and moves all sites of an operator at once.
- No existing signature changed; the new function is the only addition to the interface.
- Costs and limits:
  - `is_point_group_symmetry` now spends about 0.2 ms on setup per call. Groups with fewer than about 10 operators, and early exits on sets that are not closed, are slower per call: C2 goes from under 0.1 ms to 0.2 ms, and a 2127-operator helix that exits early from 0.6 to 9.6 ms. Every search measured is faster overall.
  - `score_ncs_in_map_point_group_symmetry` no longer accepts a `flex.int` map: `flex.linear_correlation` rejects the int values. No caller passes one; all maps come from `map_manager` as `flex.double`.

Measured on macOS arm64, without a profiler:

| | before | after |
|---|---|---|
| `is_point_group_symmetry`, 50 groups (C2-C14, D2-D14, T, O, I) | 0.51 s | 0.15 s |
| `score_ncs_in_map_point_group_symmetry`, 51 candidates on 100 + 1100 sites | 7.5 s | 0.11 s |
| `find_map_symmetry`, `generate_map(d_min=3)` map | 8.6 s | 1.2 s |
| `find_map_symmetry`, phenix_regression `D7.ccp4` / `C4.ccp4` | 2.8 / 2.7 s | 1.3 / 1.3 s |
| `tst_map_model_manager_2.py` | 49 s | 10 s |

**Results are unchanged.** I recorded, before and after the change, every `is_point_group_symmetry` result and every score (`repr` of the floats) from these runs:

- `find_map_symmetry` on the `generate_map` map;
- `find_map_symmetry` on phenix_regression `C4.ccp4`, `D7.ccp4` (symmetry `ALL` and `D7`) and `helix.ccp4` (helical);
- 1300 generated, offset, perturbed and subset groups.

Both recordings are identical, and so are the symmetry found, the operators, the centers and `ncs_cc`.

`values_at_closest_grid_points` gives the same values as `value_at_closest_grid_point` on 2.4M sites. They include exact half-grid ties, padded grids and coordinates up to 1e10.

A possible follow-up, outside this change: `cctbx/crystal/super_cell.py` (#1230) has its own inline closest-grid lookup. It uses C-style `%`, so negative fractional coordinates give negative indices. It could call the new function instead.

## Tests

- `mmtbx/regression/tst_find_ncs.py`, new `tst_08`. It checks:
  - the rotation, absolute and relative translation tolerances, each just inside and just outside its limit;
  - that the relative limit uses the mean of the two translations;
  - `symmetry_to_match`;
  - `rel_tol_t=None` when the absolute test suffices;
  - a D3 group off the origin, so the order of the product matters;
  - a 101-operator group compared in several blocks, where only the last block holds products that miss.

  It passes with the old and the new implementation. Mutants of the new code fail it: no relative test, no absolute test, ignoring `symmetry_to_match`, the wrong product order, the wrong mean, comparing only the first block, skipping the last block, and wrong block bookkeeping.
- `cctbx/maptbx/boost_python/tst_maptbx.py`, new `exercise_values_at_closest_grid_points`. Hand-derived values cover rounding, half-grid ties, negative and wrapped coordinates, padded grids, and coordinates past 2**31 grid points. Random sites are compared with the scalar lookup, and a non-3-D map must be refused. Before the function existed, the test failed with an AttributeError. Tie, wrap and padding mutants fail it.
- Suites run on macOS arm64:
  - `libtbx.run_tests_parallel` for iotbx: 213 tests, 0 failures.
  - cctbx: 160 tests, 0 failures.
  - mmtbx: 579 tests, 1 failure. `tst_add_arrows_on_plot.py` fails on a Ramachandran plot-image histogram, and it calls none of the changed functions.
- phenix_regression tests that reach the symmetry search or the NCS point-group code: 22 of 24 pass. They include `segment_and_split_map/tst_map_symmetry`, `tst_sequence_from_map*`, the NCS wizard tests, `tst5_phenix_map_box`, `combine_models` and their cif twins.
  - The 2 failures are `tst_segment_and_split_map.py` and its cif twin, both in `tst_05`, an auto_sharpen case with no symmetry. A region ordering and two region volumes differ, and that case calls none of the changed functions.

## Coding agent

| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (claude-opus-5-5) |
| Harness | Claude Code (VS Code extension) |
| Rounds of adversarial review | 3 |
| Reviewer model, version, and harness | Claude Opus 5.5 (claude-opus-5-5), a fresh Claude Code subagent each round (final: round 3) |

<details>
<summary>Final review round</summary>

~~~~~~~~
Review round 3 of branch `map_symmetry_search_speed` (b50d825dad, 22b063f6b3) against origin/master. I found no high or medium findings and four low ones. I edited nothing in the repository, `git status` is clean, and `find_clutter` on the five changed files reports nothing. My scripts are in [redacted]

**Equivalence checks: no differences on realistic inputs.** The old code was copied from `git show origin/master` into `old_code.py`.
- **`is_point_group_symmetry`, old vs new (`diff_pgs.py`):** 1749 cases, 0 differences in the result and in the attributes set on success (`_have_point_group_symmetry`, `tol_r`, `abs_tol_t`, `rel_tol_t`). The cases:
  - all `get_ncs_list` groups C2–C19, D2–D19, T, O, I at three centres;
  - perturbed copies near the tolerances;
  - groups with one operator dropped;
  - `symmetry_to_match`;
  - non-default and zero tolerances.
- **`score_ncs_in_map_point_group_symmetry` (`diff_score.py`):** 68 comparisons on random maps (an orthogonal and an oblique cell) with 300 sites in [-1, 2), 0 differences. The CC values are bit-identical.
- **End to end, `find_map_symmetry` (`e2e2.py`):** on phenix_regression `D7.ccp4` (finds D14, 28 operators, cc 0.91514801…) and `helix.ccp4` (include_helical, min_ncs_cc=0.75), the operators, cc and the full log text are byte-identical. Times, old → new: D7 2.83 → 1.39 s; helix 19.6 → 14.3 s; generate_map(d_min=3) 6.7 → 1.24 s.
- **Large groups, old → new:** C500 18.7 → 6.5 s; a 1000-operator helix 0.29 → 0.11 s. The only slowdown is the per-call setup already accepted (C2: under 0.1 ms → 0.2 ms per call).
- **Commit-message claim checked:** "50 generated point groups (C2–C14, D2–D14, T, O, I)" is correct; `get_ncs_list` returns 50 objects for those symbols.

**Low findings**

1. `mmtbx/regression/tst_find_ncs.py:1415-1469` (tst_08): the test does not check the order in which two operators are multiplied.
   - **Defect:** every group in it has rotations that commute, or translations of zero, so computing r*r1 instead of r1*r passes.
   - **Scenario:** with the product line in `ncs.py:3081-3082` mutated to compute r*r1 while keeping the translation as r1*t+t1, tst_08 and the whole of tst_find_ncs.py still print OK. The same mutant changes 181 of the 1749 realistic cases; for example D3 from `get_ncs_list` about (10.3, 20.1, -5.7) is reported as not a point group.
   - **Fix:** one assertion on a dihedral group about an off-origin centre would catch it.
   - **Verified:** `mut.py` (tst_08 alone), `mut_full.py` (the whole file), `mut_diff.py` (the 1749 cases).
   - tst_map_model_manager_3 also passes under this mutant. tst_map_model_manager_2 had run about 8 CPU-minutes under it (about 10 s normally) without finishing when I stopped it.

2. `tst_find_ncs.py:1443-1448` and `1455-1468`: two test comments describe more than the assertions check.
   - **"accepted if … e <= 5% of the mean translation":** mutants that use only the product's translation, or only the target's, instead of the mean, both survive. Cases (200, 5) and (200, 12) cannot tell them apart; e.g. t_x=200 with e=10.2 would.
   - **"the only products equal to rz(1)*rx come from the second and third blocks":** a mutant that never compares the final partial 1-product block survives. The failure is still found in block 2 when the outer operator is rx, so block 3 is never checked on its own.
   - A mutant changing `>` to `>=` on tol_r also survives, because the delta cases are not at the exact limit.
   - **Verified:** `mut.py` and `mut_full.py`.

3. `mmtbx/ncs/ncs.py:3096`: `rel_tol_t=None` now raises where the old code returned True.
   - **Defect:** the new code always evaluates `rel_tol_t * dd2`. The old `dd>abs_tol_t and dd>rel_tol_t*dd2` short-circuited, so it never touched `rel_tol_t` while every translation difference stayed within `abs_tol_t`.
   - **Scenario:** a user sets the float PHIL `rel_tol_t=None` (segment_and_split_map `reconstruction_symmetry.rel_tol_t`, or phenix.find_ncs `ncs_parameters.rel_tol_t`). D7 or I about (50, 60, 70) then gives old `True`, new `TypeError: unsupported operand type(s) for *: 'NoneType' and 'double'`. `tol_r=None` and `abs_tol_t=None` raise TypeError in both versions.
   - **Consequence:** the commit-1 claim "results are bit-identical" has this exception.
   - **Verified:** `edge_pgs.py`.

4. `cctbx/maptbx/segment_and_split_map.py:3490-3491`: an integer map now fails in the scoring function.
   - **Defect:** the old code appended values into a `flex.double`. The new code returns `map_data.select(...)`, which is a `flex.int` for an int map, and `flex.linear_correlation` refuses it.
   - **Scenario:** a C3 group on a `flex.int` map gives old `(0.663…, 0.609…)`, new:
     ```
     ArgumentError: Python argument types in
         linear_correlation.__init__(linear_correlation, int, int)
     did not match C++ signature:
         __init__(_object*, scitbx::af::const_ref<double, scitbx::af::trivial_accessor> x, scitbx::af::const_ref<double, scitbx::af::trivial_accessor> y, double epsilon=1e-15, bool subtract_mean=True)
     ```
   - **Reach:** no current caller passes an int map. Every path into this function (`get_ncs_from_map`, `optimize_center_position`) takes density maps from `map_manager`, which documents `flex.double`; `sites_orth` is always a `flex.vec3_double` from `get_points_in_map`.
   - **Verified:** `int_map.py`.

**Checked and found correct**
- The numpydoc docstring of `values_at_closest_grid_points` (types and padded-map behaviour are right).
- All inline comments in the diff ("summed in the order of matrix.sqr", the block/pair indexing).
- My hand derivation of every expected value in `exercise_values_at_closest_grid_points`, and of each tst_08 tolerance case.
- Test registration in `cctbx/run_tests.py:49` and `mmtbx/run_tests.py:272`.
- Both commit messages: length and scope follow CONTRIBUTING.md, with no attribution or tracker keys.

I don't re-raise any answer from the earlier rounds.
~~~~~~~~

</details>

Final-round findings (all low):

1. **`tst_08` did not check the order of the operator product.** Fixed: added a D3 group about an off-origin center.
2. **Test comments said more than the assertions checked** (the mean of the two translations, the last block). Fixed:
   - The cases are now e = 10.2 / 10.4 at |t| = 200, which tell the mean apart from either translation alone.
   - The block case is now a 2-fold on an oblique axis whose missing products appear only in the last block.
   - Left on purpose: `>` versus `>=` at the exact tolerance is not tested, because CONTRIBUTING.md says to keep test inputs away from thresholds.
3. **`rel_tol_t=None` raised where the old code returned True.** Fixed: the relative test runs only where the absolute one fails, as in `is_same_transform`. `tst_08` covers it.
4. **A `flex.int` map now fails in the scoring function.** Left: no caller passes one, and it is stated under Changes.

## Checklist
- [x] Searched open and closed PRs and issues for prior work: none found; related: #1230 (`super_cell.py` inline lookup)
- [x] New or changed tests are registered in the module's `run_tests.py` (both files were already registered)
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] Agent-written code: the writing agents had `AGENTS.md` and
      `CONTRIBUTING.md` in context, and the reviewing agents had
      `AGENTS.md`
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `68ae5b9fb614`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.