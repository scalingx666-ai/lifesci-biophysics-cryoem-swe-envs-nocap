# Issue

**Title:** Fix MolProbity JSON export: planarities without outliers, RNA after pickling, rna_validate_bonds json=True

## Description

(JSON export of MolProbity validation results). This fixes three bugs that crash or produce invalid JSON on current master:

1. **`restraints.planarities.as_JSON()` raises `KeyError: ''` when a model has no planarity outliers.** `n_outliers_by_model` was only initialized inside the outlier branch; bonds/angles/dihedrals already initialize it for every restraint (3581799819). Most well-refined models hit this, e.g. 1CRN, 1UBQ, 1MBN.
2. **`rna_validation.as_JSON()` raises `AttributeError` after unpickling**, e.g. with `molprobity.molprobity pickle=True`. `slots_getstate_setstate` only pickles `__slots__`, and `rna_bonds`/`rna_angles` (`n_outliers_{small,large}_by_model`) and `suitealyze` (`model_list`, `chain_list`) keep those outside `__slots__`. They are now declared in `__slots__`, as `rna_puckers` already does. Any model with RNA hits this, e.g. 1EHZ.
3. **`molprobity.rna_validate_bonds json=True` pprints a Python dict** (single quotes, `True`/`None`), which is not JSON. It now prints `json.dumps()` to the program logger like the other `rna_validate` programs.

One commit per fix, each with a regression test.

## Tests

- `tst_restraints.py`: new `exercise_json_no_planarity_outliers` (1yjp: no planarity outliers, 13 planes counted by hand from the monomer library).
- `tst_rna_validate.py`: `pickle_unpickle()` now also compares `as_JSON()` before and after the round trip; new `exercise_rna_validate_bonds_json`.
- Each new test fails on master with the error above and passes with the fix; each commit passes on its own.
- 71 registered mmtbx tests related to validation, RNA, suitename and molprobity, run in a MolProbity bootstrap build of master (9647dd1): identical results with and without this change (66 pass; 5 fail in both because that build lacks ksdssp and phenix_regression data).
- `libtbx.find_clutter` is clean on the changed files.
- End to end: `molprobity.molprobity pickle=True` on 1CRN, 1UBQ, 1MBN, 3NIR, 1HAO, 9A78, 1D3Z (10 models), 1EHZ and a modified 1CRN, then `as_JSON()` on every validation in the pickle: 5 of 91 calls fail on master, 0 with this change. RNA summaries from the pickle match `molprobity.rna_validate json=True`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a9d4a640fad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.