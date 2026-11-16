# Issue

**Title:** Charge parameters and alchemical fixes

## Description

### Charge parameters in the parameter API
`extract_parameters`, `ParameterPlan` and `inject_gradients` now handle atom charges.

- Charges are keyed by a new `AtomData.charge_type`; mass, σ and ϵ stay keyed by atom type. An empty `charge_type` means the atom type is used.
- System setup fills `charge_type` from the force field (new keyword `charge_types=:auto`). With `:auto` it is the atom type where every atom of that type has the same charge in the force field; otherwise it is a residue-template label such as `ALA/HC` or `ALA/CT/CA`. Force fields that assign charges per residue template atom are therefore supported.
- Injected charges update the cached charge sums of a `PME` with `fixed_charges=true`.
- If the atoms sharing a key have different values (e.g. masses after hydrogen mass repartitioning, or the zero-mass B copies in dual topology), the key is left out of `extract_parameters` and refused by `ParameterPlan`. These values used to be silently overwritten. Left-out charge/σ/ϵ keys are reported according to a new `strictness` keyword.
- Single-topology systems now raise a clear error in these functions; dual-topology and absolute systems are supported.

### Alchemical fixes
- `AbsoluteFESystem`/`RelativeFESystem` now support `DoubleExponential`, which becomes `DoubleExponentialSoftCore`.
- The Gapsys Coulomb soft cores no longer give `σQ` units on unitless systems, which made such systems fail when evaluated.

### Breaking
- `AtomData` has a new field, so code outside Molly that constructs it positionally must be updated.

### Tests
- `test/gradients_2.jl`: round trips; injected charges against systems rebuilt from edited force fields (garnet ethanol, ff99SBildn protein, RF and PME); Enzyme against finite differences for charges (RF and PME); alchemical injection; charge-type labelling and the left-out-key behaviour.
- `test/free_energy.jl`: unitless alchemical systems; DEXP end states for absolute and relative systems; dual-topology parameters.

🤖 Generated with the help of [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7109a1d295e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.