# Issue

**Title:** Long-range dispersion correction for the double exponential potential

## Description

**Summary**
- **New `DEXPDispersionCorrection` and `DEXPDispersionCorrectionλ`.** The tail is computed in closed form. Supports `DistanceCutoff`, `ShiftedPotentialCutoff` and `PolynomialCutoff`, with the virial. The λ version uses the soft-core α_s and β_s of `DoubleExponentialSoftCore`, which change the potential at every distance. `AbsoluteFESystem` and `RelativeFESystem` convert to it, and `set_lambda!` rebuilds it.
- **Setup.** DEXP (`CustomNonbondedForce`) force fields now get the correction, **off by default as to match OpenMM**, contrary to LJ. OpenMM applies no correction to a `CustomNonbondedForce` read from XML, which the docs now note. The error for an unsupported `lj_cutoff` now suggests `dispersion_correction=false`.
- **Parameter injection.** `inject_gradients` now recomputes the dispersion corrections, LJ and DEXP, from the injected σ and ϵ (and α and β), through a new `update_cached_parameters` hook. Their gradients are therefore included. The DEXP corrections share the `inter_DEXP_alpha` and `inter_DEXP_beta` keys with the pairwise term. To support this, the LJ corrections store their class-pair structure as new struct fields, and the λ classes are now keyed by atom type as well.
- **Enzyme fix.** `potential_energy` summed the general interactions in a loop over a tuple of different types. For PME plus any dispersion correction (LJ included) that produced a `Union`, and Enzyme failed with `IllegalTypeAnalysisException`. It now uses `map` over the tuple. The results are unchanged.

**Tests**
- Compared with OpenMM's numerically integrated correction. Energy and virial checked against quadrature for all three cutoffs.
- λ correction checked against brute-force integrals of the soft-core pair potentials.
- Injected parameters checked against a system rebuilt from a modified force field. Enzyme gradients checked against finite differences, for LJ, DEXP and their λ versions.
- Test changes:
  - The OpenFE scheduler and the single-topology relative end states now check the correction separately from the rest of the energy, because of conventions already shared with LJ.
  - The Garnet energy-reference test passes `dispersion_correction=false`.
- Not run: the full suite and GPU.

## 🤖 Implemented with [Claude Code Opus 5.5](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0b7fdeca9ee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.