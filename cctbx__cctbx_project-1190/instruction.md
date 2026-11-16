# Issue

**Title:** VDW/neutron fixes: element-based VDW fallback, neutron distances to ener_lib and Optimizer

## Description

## Summary
Three small robustness/correctness fixes for VDW radii and neutron distances in the probe2 / reduce2 path.

1. **Element-based VDW fallback for unparameterized atoms** (`mmtbx/probe/Helpers.py`). When `getExtraAtomInfo()` can't look up an atom (e.g. an unknown ligand whose `type_energies` were never assigned during interpretation), `vdwRadius` defaulted to 0, which crashes probe2. It now falls back to the element-based radius from `ener_lib` (N=1.55, C=1.70, O=1.40, …) with a warning, so probe2 can still run contact analysis on structures containing unknown ligands.

2. **Pass `useNeutronDistances` to `ener_lib` in `getExtraAtomInfo`** (`mmtbx/probe/Helpers.py`). The `ener_lib` constructor wasn't receiving the flag, so it always used X-ray VDW radii even when nuclear distances were requested.

3. **Pass `useNeutronDistances` to the reduce2 Optimizer** (`mmtbx/hydrogens/__init__.py`). `place_and_optimize_hydrogens` was passing `nuclear` to `place_hydrogens` but **not** to `Optimizers.Optimizer` (which accepts `useNeutronDistances` and defaults it to False), so neutron-distance models were optimized with X-ray distances. Now the helper forwards `useNeutronDistances=nuclear`.

## Note on commit 3's location
The original change targeted the inline Optimizer call in `clashscore2.check_and_add_hydrogen`. Master has since refactored that function to delegate to the shared `mmtbx.hydrogens.place_and_optimize_hydrogens`, so the fix is applied there instead (one place, shared by clashscore2, the kinemage view, and refinement).

## Tests
- `mmtbx/probe/tst_mmtbx_probe_ext.py` passes.
- `mmtbx.clashscore2` on a hydrogenated model: X-ray default unchanged (`clashscore = 293.67`); `nuclear=True` runs cleanly and now differs (`303.49`), confirming the neutron flag flows through.
- `mmtbx/kinemage/tst_validation.py` still passes (it also uses `place_and_optimize_hydrogens`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4ade5dd0171`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.