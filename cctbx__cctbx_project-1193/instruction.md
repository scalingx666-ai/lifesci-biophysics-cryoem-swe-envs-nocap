# Issue

**Title:** Fix water/ligand classification and single-atom het rendering in kinemage

## Description

## Summary

Two small fixes to how non-polymer residues (heteroatoms, ions, waters) are rendered in
the MolProbity kinemage output.

## What changed (`mmtbx/kinemage/validation.py`)

1. **Water and ligand classification** (`_draw_residue_bonds`, `get_kin_lots`):
   - Classify waters by residue class (`res_class == "common_water"`) instead of
     `resname == 'hoh'`, so `WAT`/`DOD` waters are drawn as water balls rather than pink
     het bonds.
   - Make het-bond drawing the `else` fallback, so non-polymer classes that aren't `other`
     or `common_small_molecule` (e.g. `common_saccharide` — NAG, MAN, …) get their bonds drawn.

2. **Single-atom het residues** (`get_kin_lots`): draw any single-atom non-polymer residue
   as a sphere (not just `res_class == "other"`). This covers cases like a lone `S` from an
   `SO4` that would otherwise be dropped (as happens in 1nxb).

## Testing

- New `exercise_het_ion_water` test in `tst_validation.py` exercises ions (ZN), an SO4
  ligand, `HOH`/`WAT` waters, and a single-atom SO4; the full `tst_validation.py` passes.
- `phenix_regression/validation/tst_kinemage.py` passes.
- Functional check on 1nxb: waters render as balls, the single-atom SO4 now appears in the
  ion spherelist, and probe-dot output is unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87193eb43e2a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.