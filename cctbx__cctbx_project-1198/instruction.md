# Issue

**Title:** Kinemage: build_kinemage_from_model helper, ribbon polish, and cablam/coil display toggles

## Description

## Summary

Follow-on kinemage work building on the ribbon-rendering PR (#1197). This bundles
the high-level `build_kinemage_from_model` helper together with the ribbon polish
and display-toggle fixes, because the toggles thread through the helper and the two
cannot be cleanly separated.

Five commits (all in `mmtbx/kinemage/` plus the cablam wheel gating):

1. **Fix errant ribbon triangle and rename ribbon master controls** — remove the
   `i == 0` short-circuit in ribbon element discovery that produced a duplicate
   1-unit sheet element when a chain starts on a SHEET residue (visible in 1ubq).
   Rename the ribbon masters `{protein}`/`{nucleic acid}` → `{protein ribbon}`/
   `{NA ribbon}` so they no longer collide with the molecular-representation masters.

2. **Add `build_kinemage_from_model` high-level helper** — a single entry point that
   takes an `mmtbx.model.manager`, runs (or accepts pre-run) validators, computes the
   secondary-structure annotation and probe dots, and assembles the kinemage. Wraps the
   `i_seq_name_hash` / `bond_hash` / `ss_bonds` / `sites_cart` plumbing that
   `_build_kinemage` requires.

3. **Add `include_cablam_wheels` and `plain_coils` toggles** — the cablam score wheels
   are gated behind `include_wheels` in `cablamalyze.as_kinemage` (default `True` for
   standalone use) and `include_cablam_wheels` in the builders (default `False` for
   multicrit viewers, where they are too noisy). `plain_coils` suppresses the rear
   deadblack halo behind coil ribbons. Both flags are threaded through `_build_kinemage`,
   `build_kinemage_from_model`, `make_multikin`, and `export_molprobity_result_as_kinemage`.

4. **Pass `restraint_objects` to probe sub-models** so custom ligand restraints are
   honored during probe-dot generation.

5. **Fix ribbon rendering: NA as sheet, 3-residue minimum, short-element filter** —
   nucleic-acid ribbons now render as SHEET (flat with arrowheads), matching Prekin's
   BETA behavior; the minimum contiguous segment is raised to 3 residues (matching
   Prekin's `fragment.number > 5`), fixing crashes from 1-residue segments and removing
   2-residue stubs; degenerate/short HELIX/SHEET elements are demoted to COIL.

## Testing

- `mmtbx/kinemage/tst_validation.py`: all 20 exercises pass, including the new
  `build_kinemage_from_model` helper/hydrogens/toggle tests and the ribbon tests.
- phenix_regression `validation/tst_kinemage.py`: OK.
- `phenix.kinemage` and `mmtbx.kinemage.validation.run()` on 1nxbH: 3481 greentint
  probe dots (unchanged vs master), ribbons render with the renamed masters, cablam
  wheels off by default in the assembled multicrit output.
- `mmtbx.ribbons` standalone program: renders with the new masters.
- New builder parameters all default to backward-compatible values; the existing
  `export_molprobity_result_as_kinemage` caller in `mmtbx/command_line/molprobity.py`
  is unaffected.
- `libtbx.find_clutter` and `py_compile` clean on all changed files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5733a6dfffe6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.