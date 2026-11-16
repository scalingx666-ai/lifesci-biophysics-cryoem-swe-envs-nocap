# Issue

**Title:** Locked-schema completion aliases drift only, never another quantity

## Description

## What this changes

When a hyperspectral series re-runs a dataset with fresh code (a new regime's anchor, or a refit), the column names can drift a little: a unit added to the end (`..._mean_eV`), or a prefix on a diagnostic map (`Fit_R2_mean`). The series fixes that by renaming the drifted column to the name the rest of the series uses.

The renaming rule was too loose. It took any single column whose name contained the expected name, so a column such as `Band2_Depth_uncertainty` could be filed as `Band2_Depth`, and the uncertainty went into the feature table as if it were the measured depth.

Now only real drift is renamed:
- a units suffix on **one** side, read from that column's own record (two different units, such as nm and eV, are two different numbers and are never merged);
- a prefix with no uncertainty or noise word in it.

A column that names a different quantity (an uncertainty, an error, a signal-to-noise ratio, a smoothed version) or is in a different unit keeps its own name, and the missing column is reported as a gap rather than silently filled.

One limit, stated plainly: a prefix that names another quantity by some other word (`Band2_Depth` against `Depth`) is still renamed, as before. Only the listed uncertainty and noise words are recognised.

---

## Technical details

- `hyperspectral_series.complete_locked_schema(row, locked_columns, logger, locked_units=None)`: a candidate must pass `_same_quantity`:
  - if both sides carry units and they differ, it is another number (no alias);
  - otherwise each side's own units suffix is stripped (`_column_units(row)` from the row's `feature_records`; `locked_units` from the schema source's records), and only an exact match, or a prefix none of whose **words** is a quantity word (`err`, `error`, `uncert`, `uncertainty`, `unc`, `std`, `sigma`, `snr`, `noise`, `ci`, `var`, …), passes;
  - words are those of the original name, split at separators and at case changes, and a listed word is recognised up to its digits and a plural (`CI95`, `Uncertainties`); names still compare normalised, so `Fit_R2` and `FitR2` are the same, while `Specific_Area` no longer matches `ci` and `Cis_` is not a plural of it.
  With no units known for a side, a suffix on that side is not drift.
- The series driver records the schema source's per-column units as `schema["units"]` and passes them to every completion call.
- Tests (`test_hs_series.py`):
  - the issue's case (`Band2_Depth_uncertainty` stays under its own name; `Band2_Depth` is a gap);
  - the other quantity suffixes and prefixes, units drift on one side, an innocent prefix;
  - two different units (nm/eV, eV/meV, counts against counts/s) as gaps;
  - whole-word quantity words;
  - through the real series driver, a second regime's fresh anchor reporting in eV against a first regime in nm, which gives a gap and no alias. The existing aliasing test now passes the locked units the driver records; one expectation changed on purpose: a `_smoothed` sibling no longer makes the units-dropped `Width_mean` ambiguous, so it is aliased like `L3_Position_mean`.
- CLAUDE.md: one sentence.

**Live check (Bedrock, real data).** A four-cube hyperspectral series was run, and the planner split it into two regimes. The second regime's anchor was regenerated under locked targets: fresh code, with names fixed by the first dataset. The completion then did the following:
- It aliased the one units drift: the anchor reported `N_Pixels_Fit`, and the locked column `N_Pixels_Fit_count` carries the unit "count" in its own record.
- It reported the six quantities that regime did not produce as `locked_schema_gap`.
- Uncertainty columns (`…_err_…`) elsewhere in the series stayed under their own names.

All four datasets succeeded.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55b86f181ec8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.