# Issue

**Title:** clashscore2: add ignore_missing_restraints for models with unknown ligands

## Description

## Summary

`clashscore2` aborts with a `Sorry` when a model contains a residue with no restraints
(e.g. an unknown ligand), so the whole contact analysis fails because of one problem
residue — even though the rest of the structure is perfectly analyzable.

This adds an **`ignore_missing_restraints`** option (phil parameter on the program, keyword
on `clashscore2` and `check_and_add_hydrogen`) that continues on a best-effort basis:
residues without restraints simply get no hydrogens added, while their heavy atoms still
take part in the contact analysis. **Default `False`, preserving current behavior.**

The naming and phil style mirror the existing `ignore_missing_restraints` in
`mmtbx/programs/reduce2.py`.

### Both gates have to open

`check_and_add_hydrogen` already delegates to `mmtbx.hydrogens.place_and_optimize_hydrogens`
but did not forward `raise_on_missing`. That alone turns out to be insufficient — there are
two checks in series:

1. `raise_on_missing` → *"Restraints were not found for the following residues"*
2. `stop_for_unknowns` → *"Fatal problems interpreting model file: … unknown nonbonded energy
   type symbols"*, in the re-process that follows

Relaxing only the first still aborts the run on the second, just with a different message.
So best-effort mode relaxes both. This mirrors the pairing already used by
`mmtbx/programs/validate_ligands.py:150-158`, which passes `stop_for_unknowns=False` and
`raise_on_missing=False` together.

### Compatibility

The 2-tuple return of `check_and_add_hydrogen` is **unchanged**, so all existing callers
(`clashscore2`, `undowser2`, `tst_reduce_switch`) are unaffected — the new parameter is
purely additive and keyword-only in practice.

## Testing

- New `test_clashscore2_ignore_missing_restraints` in `mmtbx/regression/tst_reduce_switch.py`:
  a model with an unknown ligand raises `Sorry` by default and completes with
  `ignore_missing_restraints=True`. (The fixture uses `UNL` specifically — many made-up
  three-letter codes resolve to some monomer entry and would not exercise this path at all.)
- Full `tst_reduce_switch.py` passes, including `test_clashscore2_delegates`, which pins the
  2-tuple return — unchanged, demonstrating no API break.
- `mmtbx/validation/regression/tst_undowser2.py` passes (indirect caller).
- End-to-end: `clashscore2(..., ignore_missing_restraints=True)` on an unknown-ligand model
  completes and yields the usual `flat_results` / `summary_results` JSON plus a populated
  `hydrogenated_model`; with the default it still raises `Sorry`.
- No change on normal models: `mmtbx.clashscore2` on 1nxbH gives clashscore 294.57 both with
  and without this patch.
- `libtbx.find_clutter` and `py_compile` clean.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c6a173b56fdc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.