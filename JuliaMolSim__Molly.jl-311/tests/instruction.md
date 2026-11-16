# Issue

**Title:** Reaction field exclusion correction and self terms

## Description

The reaction field in Molly currently follows OpenMM where excluded pairs feel no reaction field at all. In other simulation engines such as GROMACS the dielectric continuum beyond the cutoff responds to all charges within it, so excluded pairs also get the reaction field term, without direct Coulomb, and each atom gets a self term.

This PR adds that correction as an optional specific interaction that can either be added manually or via `SetupCoulombReactionField`.

### Changes
- `CoulombReactionFieldExclusion(; self=false)`: new specific interaction. Within the cutoff $r_c$, an excluded pair gets

  $$V(r_{ij}) = \frac{q_i q_j}{4 \pi \varepsilon_0 \varepsilon_r} \left( k_\mathrm{rf} r_{ij}^2 - c_\mathrm{rf} \right)$$

  and a self entry gets $V_\mathrm{self} = -\frac{q_i^2}{4 \pi \varepsilon_0 \varepsilon_r} \frac{c_\mathrm{rf}}{2}$ and no force, with $k_\mathrm{rf}$ and $c_\mathrm{rf}$ as in `CoulombReactionField`. Excluded pairs with $r_{ij} > r_c$ contribute nothing.
- `CoulombReactionFieldExclusionData`: holds the cutoff, dielectrics, Coulomb constant and λ scheduler once per list, like `EwaldExclusionData`. Can be built from any of the four reaction field interactions.
- `reaction_field_exclusions(inter, eligible, special)` / `reaction_field_exclusions(inter, n_atoms, pairs)`: helpers to set up the list of excluded pairs plus one self entry per atom for systems built by hand.
- new `exclusion_correction` param for `SetupCoulombReactionField`: when `true`, systems built from files get the list automatically. It raises an error with `NoNeighborFinder` which would cause excluded pairs to be counted twice.
- Parameter injection: `inject_gradients` passes injected reaction field parameters (`inter_CO_*`) on to the exclusion data, so the energies and gradients for those keys include the correction.
- Free energy: handled the same way as `EwaldExclusion`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `38fc9250a7f6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.