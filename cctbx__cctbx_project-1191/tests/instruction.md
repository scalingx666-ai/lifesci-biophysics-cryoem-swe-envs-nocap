# Issue

**Title:** kinemage: move generation into per-validator as_kinemage() + probe-dot reuse

## Description

## Summary

Moves MolProbity kinemage generation out of standalone wrapper functions in
`mmtbx/kinemage/validation.py` and into per-validator `as_kinemage()` methods, so each
validator owns its own kinemage output, and adds a reusable probe-dot path.

Three focused commits:

1. **Move kinemage generation into individual validation classes.** Adds `as_kinemage()`
   methods to `rotalyze`, `ramalyze`, `rna_puckers`, and `cablam`. Removes the old
   `rotamer_outliers()`, `rama_outliers()`, and `pperp_outliers()` wrapper functions from
   `kinemage/validation.py` (and their now-unused imports). `_build_kinemage`,
   `make_multikin`, and `export_molprobity_result_as_kinemage` now call those methods and
   wire in omega, RNA-pucker, and CaBLAM results.

2. **Fix Rama outlier kinemage markup to draw from CA midpoints** — the highlight now spans
   only the half-region around the central residue, matching the old MolProbity/prekin style.

3. **Add `make_probe_dots_from_model()`** to reuse an already-hydrogenated model for probe
   dots (skipping reduce2/Optimizer), plus a `probe_dots_kin` param on `_build_kinemage` to
   accept pre-computed dots, and save the hydrogenated model on `clashscore2` for downstream
   reuse. The default path is unchanged (`probe_dots_kin=None` → `make_probe_dots`).

## Notes

- Ribbon rendering and secondary-structure (`ss_bonds`/`ss_annotation`) plumbing that
  accompanied this work on the source branch are intentionally **deferred to a later PR**;
  this PR is scoped to the `as_kinemage()` migration + omega/RNA/CaBLAM + probe-dot reuse.

## Testing

- `mmtbx/kinemage/tst_validation.py` — all groups OK.
- phenix_regression `validation/tst_kinemage.py` — OK (probe dots non-empty, 69 greentint
  on the test structure).
- `tst_rotalyze`, `tst_ramalyze`, `tst_cablam`, `tst_rna_validate` — OK.
- `phenix.kinemage` on a full model (1nxbH) renders all sections (Rama/Rota/Cbeta/CaBLAM/
  omega) with non-empty probe dots.
- `libtbx.find_clutter` clean (exit 0); `py_compile` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4ade5dd0171`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.