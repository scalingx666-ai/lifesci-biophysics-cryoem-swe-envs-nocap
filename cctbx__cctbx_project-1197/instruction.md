# Issue

**Title:** Extract ribbon rendering into a reusable module and add to MolProbity kinemage

## Description

## Summary

Extracts the ribbon-rendering logic from the `mmtbx.ribbons` program into a reusable
library module (`mmtbx/kinemage/ribbon_rendering.py`) and wires it into the MolProbity
kinemage output, so ribbons can be drawn as part of the integrated kinemage (off by
default). `mmtbx/programs/ribbons.py` is refactored to call the extracted code.

## What changed

**New `mmtbx/kinemage/ribbon_rendering.py`** — the ribbon-drawing logic previously living
as instance methods on `mmtbx.ribbons`'s `Program` class, lifted into module-level
functions (`spline_interpolate`, `consolidate_sheets`, `get_point_id`,
`format_ribbon_point`, `render_fancy_ribbon`, `generate_chain_ribbons`, plus
`build_secondary_structure_map` and small `Crayon`/`Range`/`RibbonElement` helpers). The
algorithm is unchanged.

**`mmtbx/programs/ribbons.py`** — the inline ribbon methods are removed and the program now
imports/delegates to `ribbon_rendering.py`. **`mmtbx.ribbons` output is byte-identical
before/after this change** (verified on a real structure), so this is a pure refactor.

**`mmtbx/kinemage/validation.py`** — `_build_kinemage` gains an `ss_annotation` parameter and
emits a `{ribbon}` subgroup (via `generate_chain_ribbons`) within the main model group;
`make_multikin` computes the secondary-structure annotation and threads it through;
`export_molprobity_result_as_kinemage` gains the pass-through. Header/footer get a
`{ribbon}` master toggle — **off by default**.

**`mmtbx/kinemage/tst_validation.py`** — 10 new tests covering ribbon rendering, the
ribbon-in-main-group placement, `build_name_hash` dict format, `_same_residue`, disulfide
bonds, `_draw_residue_bonds`, backbone-atom tracking, and end-to-end `make_multikin`.

## Notes

- Ribbon rendering is wrapped in a guard so that if it can't build ribbons for a given
  chain, the rest of the kinemage is still produced. For very short chains (fewer than 3
  residues) the underlying guide-vector construction can't run, so ribbons are skipped for
  that chain (a follow-up applies a proper 3-residue-minimum filter matching Prekin).
- Ribbons are off by default; existing kinemage output is unchanged unless the `{ribbon}`
  master is toggled on.

## Testing

- `mmtbx/kinemage/tst_validation.py` (16 tests incl. the 10 new) — all pass.
- `phenix_regression/validation/tst_kinemage.py` — passes.
- `mmtbx.ribbons` on a real structure: byte-identical output vs. before (refactor is
  behavior-preserving).
- `phenix.kinemage` on a real structure: ribbon subgroup + colorsets render; probe dots,
  disulfides, and het/water output are unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ee7e80a475e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.