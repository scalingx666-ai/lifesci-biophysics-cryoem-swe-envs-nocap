# Issue

**Title:** Unit-aware `extract_parameters` / `ParameterPlan` / `inject_gradients`

## Description

The parameter API now works on unitful systems, on CPU and GPU. Previously it threw a `DimensionError`.

- `extract_parameters` returns plain numbers in the units each parameter is stored with in the system. For systems built with the default units, these equal the unitless values.
- `ParameterPlan` / `inject_gradients` take those numbers and re-attach each field's unit. A new internal function, `convert_parameter`, replaces `convert` in `inject_parameters` (both the generated method and `PeriodicTorsion`), `inject_atom` and `injected_masses`. It also accepts values that carry units and converts them.
- The unitless and Enzyme path is unchanged, because there `convert_parameter` dispatches to `convert`. Gradients are still unitless-only.
- Parameter names are unchanged.

**Tests:** new testset "Parameters with units" in `test/gradients_2.jl`, using garnet ethanol with RF on CPU and CUDA:
- injecting the extracted values unchanged gives identical atoms, interactions, energy and forces;
- one modified value each for σ, ϵ, mass, a bond k, a torsion k and DEXP α: extracting again returns the modified value, and energy and forces match a unitless copy with the same change.

`gradients_2`, `protein_1`, `protein_2` and `gradients_4` (CPU) pass. `gradients_4` on CUDA fails at master too, because of CUDACore's Enzyme rule for `CuArray` allocation; that is unrelated to this PR and has its own .

Docs: updated docstrings, a short paragraph in `differentiable.md`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40968654ea54`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.