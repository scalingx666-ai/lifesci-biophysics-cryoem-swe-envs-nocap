# Issue

**Title:** Extend script_mode to liquid, alchemy, and mode:ts

## Description

## Summary

Currently `script_mode: true` only works for `mode: fe` with `reference_phase: solid`. For other phases/modes the code unconditionally constructs a `pylammpsmpi.LammpsLibrary` — which requires LAMMPS built as a shared library with Python bindings (`PKG_PYTHON=ON`, `BUILD_LIB=ON`, `BUILD_SHARED_LIBS=ON`).

This is restrictive for users running ML interatomic potentials whose LAMMPS plugins ship only as a regular executable build (e.g. `pair_grace` against a TensorFlow SavedModel; `pair_mace`; etc.). For these the natural workflow is `script_mode: true` + `lammps_executable: <path>` — calphy emits a `.lmp` script and the user's `lmp` runs it.

This PR extends that workflow to **liquid**, **alchemy**, and **`mode: ts`** so they all honour `script_mode: true` end-to-end and shell out via `lammps_executable`.

## Changes

1. **Extend script_mode to liquid and alchemy phases** (1cd7612)
   - `helpers.py`: `LammpsScript` gains a `__getattr__` polyfill so unknown attributes (e.g. `lmp.velocity(...)`, `lmp.run(N)`) lower to the equivalent `command(...)` call. Lets the liquid/alchemy routines use the same Python-style API as the in-process `LammpsLibrary` path without each call site needing two branches.
   - `liquid.py`: propagate `script_mode` into the LAMMPS factory; emit `averaging.lmp` and `integration.lmp` at the end of each routine when in script mode.
   - `alchemy.py`: same propagation pattern.

2. **Extend script_mode to mode:ts** (76b0ad7)
   - `phase.py`: `reversible_scaling` and `temperature_scaling` were the last sites hardcoding `script_mode=False`; replace with `script_mode=self.calc.script_mode` and write the resulting `reversible_scaling_<i>.lmp` / `temperature_scaling_<i>.lmp`.

Default behaviour is unchanged: `script_mode` still defaults to `false`, all existing in-process workflows go through the same `LammpsLibrary` path as before.

## Tests

`pytest tests/` — all 96 existing tests pass on this branch.

The branch is based on commit 0911ac4; `main` has since moved 9 commits ahead (`phase_diagram.py` rework + version bump 1.7.2 → 1.7.3 + tdb test renames). None of the upstream changes touch the files modified here (`phase.py`, `liquid.py`, `alchemy.py`, `helpers.py`), so this should merge cleanly — happy to rebase if preferred.

## Out-of-band validation

Used to drive a 48-potential calphy `mode: ts` campaign computing F(T) BCC/FCC for GRACE Fe ML interatomic potentials against a custom LAMMPS binary that doesn't have Python bindings.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b3c47a0b27f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.