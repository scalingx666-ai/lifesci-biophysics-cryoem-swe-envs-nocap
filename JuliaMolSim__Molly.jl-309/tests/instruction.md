# Issue

**Title:** Keep parameter keys in alchemical systems

## Description

### Summary
`AbsoluteFESystem` and `RelativeFESystem` replace bonded and 1-4 interactions with λ versions (`HarmonicBondλ`, `PeriodicTorsionλ`, `LennardJones14Scaled`, …). None of those defined parameter metadata. So `extract_parameters` silently dropped their keys, and `inject_gradients` ignored them. Examples of keys lost: 192 for ethanol in water, 5556 for a CHARMM protein–ligand system. Any gradient with respect to a bonded or 1-4 parameter of an alchemical system was therefore lost, with no error.

### What's changed
- **Keys:** every λ type now has the keys of the type it replaces: `HarmonicBondλ`, `HarmonicAngleλ`, `CosineAngleλ`, `UreyBradleyλ`, `MorseBondλ`, `HarmonicTorsionλ`, `RBTorsionλ`, `PeriodicTorsionλ` (PT/IT), and `LennardJones14Scaled`/`SoftCoreBeutler`/`SoftCoreGapsys`. In absolute and dual-topology systems these types store the base scalars, and λ only weights the energy, so a key keeps its meaning. An alchemical system now has every key of the system it was built from. Pairwise soft cores still add their own keys (e.g. `inter_CO_α`); the 1-4 soft cores' `α` is not a parameter.
- **Single topology refused:** it stores two values per parameter. `extract_parameters` and `ParameterPlan` now raise an `ArgumentError`, as they already did for single-topology atoms.
- **Inconsistent interaction keys:** an interaction key whose interactions have different values is left out by `extract_parameters`, with a report under `strictness`, and refused by `ParameterPlan`, the same rule as for atom keys. An example is a bond type that the two end states of a `RelativeFESystem` parametrise differently. Without this, injection would silently overwrite B's values with A's. No bundled system is affected.
- **Enzyme fix:** injecting into λ types whose only non-parameter fields are `λ_mixing` and `scheduler` raised `EnzymeNoTypeError` ("cannot deduce type of copy", on `LambdaScheduler`'s `Bool` fields). This sounds like an Enzyme issue like the one I ran into a few days ago, but it was fixed with a workaround for this PR: non-parameter fields now go through an internal `copied_field`, which is the identity except that a `LambdaScheduler` is rebuilt field by field. No struct field order changed.
- **Docs:** `differentiable.md` and `free_energy.md`.

### Tests
- `test/free_energy.jl`: ethanol in water and a CHARMM TYK2 protein–ligand system, as absolute systems at λ = 0, 0.5, 1 and dual-topology relative systems. Each case checks:
  - every original key is kept, with the same value;
  - an unchanged round trip is exact;
  - changing a bonded, torsion or 1-4 key equals rebuilding from the changed original system (rtol 1e-12).

  It also covers the inconsistent-key case.
- `test/gradients_2.jl`: Enzyme against finite differences for every λ type, with a non-trivial λ weight, plus the single-topology errors.
- `gradients_2`, `free_energy`, `protein_1`, `protein_2`, `conversion` and `gradients_4` pass.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bd15c6d5ccd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.