# Issue

**Title:** Add `ThermodynamicCycle` for composing free energies from legs; fix MBAR uncertainties

## Description

### Summary
Molly's docs describe solvation and binding cycles only in prose. This PR adds a small, estimator-agnostic API to compose a free energy from legs, with an uncertainty.

```julia
ΔG_hyd = ThermodynamicCycle(
    Leg(tss_state_vac,  N => 1; coefficient=+1, name=:vacuum),
    Leg(tss_state_solv, N => 1; coefficient=-1, name=:solvated);
    correction=standard_state_correction(298.15u"K", 1u"bar"),
)
free_energy(ΔG_hyd)             # Σ cᵢ ΔFᵢ + correction, energy units
free_energy_uncertainty(ΔG_hyd) # √Σ cᵢ² σᵢ², legs assumed independent
```

### What's added
- **`Leg`**, **`ThermodynamicCycle`**, **`free_energy`**, **`free_energy_uncertainty`**. States are given by index or `ThermoState` name. Units are checked: an incompatible leg or correction raises an `ArgumentError`.
- **Leg-source interface:** `free_energy_difference(source, i, j)` and `free_energy_difference_uncertainty(source, i, j)`, both in energy units. Implemented for:
  - `TSSState`: value from `tss_free_energies`, jackknife error from `tss_free_energy_uncertainties`;
  - `AWHState`: value only, since AWH has no uncertainty estimate (the uncertainty method raises);
  - `Molly.MBARResult`, new: MBAR free energies with their asymptotic covariance, for umbrella sampling, REMD or any set of samples;
  - `PMF`: the difference between two bins.

  Any other type can be a source by adding these two methods.
- **Temperature rule:** each leg is converted with the kT of its own states. A leg whose two states differ in temperature is an error, because reduced free energies can't be converted with a single kT there. Legs at different temperatures can still be combined in one cycle.
- **`standard_state_correction(T, P; concentration=1u"mol/L")`** = RT ln(v_gas/v_std), in kJ/mol.
- **Docs:** a new "Thermodynamic cycles" section in `free_energy.md`.

### Fix: MBAR uncertainties
The covariance Θ = [(WᵀW)⁺ − N]⁺ is singular along the direction used to decide which state is taken as the zero free energy. Rounding leaves a tiny eigenvalue there, which `pinv` inverted into a huge one. With unequal sample counts and poor overlap, this understated σ:
- for `MBARResult`, by up to 3× (0.23 against an empirical 0.67);
- in the existing `pmf_with_uncertainty`, by up to about 0.6×.

Both now use one helper, `mbar_covariance`, which projects out the gauge direction first. Well-sampled, equal-count cases are unchanged.

### Tests
- New `test/thermodynamic_cycle.jl`:
  - cycle arithmetic and units on a toy source;
  - fixed `TSSState`s compared with the hand combination;
  - AWH, MBAR and PMF sources;
  - error cases;
  - the MBAR covariance against the direct form Wᵀ(I − WNWᵀ)⁺W;
  - σ calibration against the spread over independent data sets.
- `test/analysis.jl`: a PMF σ calibration test in the unequal regime.
- `thermodynamic_cycle`, `tss`, `free_energy`, `analysis` and `aqua` pass.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bd15c6d5ccd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.