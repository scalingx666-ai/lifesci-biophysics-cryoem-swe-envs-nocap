# Issue

**Title:** Use structured atom-label dict instead of pdb_label_columns slicing in kinemage

## Description

## Summary

Refactors the kinemage code to identify atoms by a structured dict of label fields
instead of slicing a fixed-width `pdb_label_columns()` string. This fixes CIF
compatibility, where chain IDs, residue names, and atom names can exceed the classic
PDB column widths and would break fixed-offset slicing.

## What changed

**`mmtbx/kinemage/validation.py`**
- `build_name_hash()` now stores a dict per atom — `name`, `altloc`, `resname`,
  `chain_id`, `resseq`, `icode` (via `atom.fetch_labels()`) — instead of the
  `pdb_label_columns()` string.
- All consumers updated to read named fields instead of fixed offsets:
  `_draw_residue_bonds`, the SS-bond drawing in `get_kin_lots`, `_build_ss_bond_list`,
  and `_build_bond_hash`.
- Adds a small `_same_residue(info_a, info_b)` helper (replaces the `[9:14]` slice
  equality used to group intra-residue bonds).

**`mmtbx/validation/cablam.py`**
- `cablamalyze.as_full_kinemage()` now uses the shared `_build_bond_hash` helper
  instead of an inline copy of the (now dict-based) bond-hash logic.

## Notes

- Blast radius is limited to these two files: `cablam.py` is the only module that
  imports `build_name_hash` from `mmtbx.kinemage.validation`. The identically-named
  helpers in `mmtbx/validation/utils.py` and
  `mmtbx/geometry_restraints/torsion_restraints/utils.py` are separate and untouched.
- The original commit also converted the `pperp_outliers`/`rama_outliers`/
  `rotamer_outliers` wrapper functions, but those were already removed upstream (their
  logic now lives in the validation classes' `as_kinemage()`), so that part is dropped.

## Testing

- `mmtbx/kinemage/tst_validation.py`, `phenix_regression/validation/tst_kinemage.py`,
  `tst_cablam.py`, `tst_cablam_2.py` all pass.
- Direct check of `cablamalyze.as_full_kinemage()` produces valid vectorlist output
  through the new dict path.
- Functional `phenix.kinemage` on 1nxb: disulfides, het/water rendering, and probe
  dots are all unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `924cb758e4e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.