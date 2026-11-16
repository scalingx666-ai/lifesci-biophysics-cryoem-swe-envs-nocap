# Issue

**Title:** Make ParticleSystem construction trim-safe

## Description

Removes the non-inferable code in the `ParticleSystem` constructors that blocks compiling packages that use CellListMap with `juliac --trim=safe`. Found while compiling Packmol.jl to a standalone executable. None of the changes break existing code.

## Changes

- **`ParticleSystemPositions.updated`** is now a concrete `Base.RefValue{Bool}` instead of the abstract `Ref{Bool}`. Every `updated[]` read and write was a dynamic call (`CellLists.jl`, `NonPeriodicCells.jl`).
- **The constructor no longer reassigns `output`.** That variable is captured by the comprehension that builds the threaded outputs, so it was boxed (`Core.Box`). With `reset = false`, `_reset_all_output!` returns `output` unchanged, so the assignment did nothing.
- **New `get_dim` method for `SVector` coordinates.** It takes the dimension from the coordinate type, so `Val(DIM)` is a compile-time constant even when the unit cell is a `Vector`/`Matrix`. The unit cell size is still checked at runtime.
- **New feature: `output_name` now also accepts a `Val`**, e.g. `output_name = Val(:forces)`. The defaults are now `Val`s, so constructing without `output_name` is fully inferable. Passing a `Symbol` works exactly as before, through a dynamic call. `InPlaceNeighborList` now passes `Val(:nb)` internally.

## Changelog and docs

- `CHANGELOG.md`: under 0.10.6-DEV, a FEATURE entry for `output_name = Val(...)` and an ENHANCEMENT entry for type-stable construction.
- Docs: new "Type-stable construction" section in `ParticleSystem/options.md` (when the constructor is inferable, with a runnable example, and why it matters for `juliac --trim`), linked from the paragraph in `single_set_simple.md` that introduces `output_name`. That paragraph's broken sentence ("the output value from the `system.output` field") is fixed too.
- The docs build succeeds locally. The only warning is the existing large-output one on `examples.md`.

## Testing

- New test item, "ParticleSystem constructor type stability": checks `@inferred` on the keyword constructor (one and two position sets; unitcell as `Vector`, `Matrix`, `SVector`, and `nothing`), that `Symbol` names still work, and that a dimension mismatch still throws.
- The full test suite passes locally (Julia 1.12).
- Trim check: I built Packmol.jl with `juliac --trim=safe`. Verifier errors raised inside CellListMap's own source dropped from 32 (v0.10.5) to 0. The remaining ones are on the Packmol side.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e9c7540bba4a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.