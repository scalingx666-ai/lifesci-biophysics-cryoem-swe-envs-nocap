# Issue

**Title:** KSPACE: fix several bugs

## Description

**Summary**

KSPACE: fix `ewald/disp` dispersion error estimators + harden `g_ewald` Newton solvers. Also make `kspace_modify mix/disp` override work for `ewald/disp` instead of being silently ignored. Fix slab correction self-energy in Ewald/PPPM dipole and spin variants, minus `pppm/dipole` which was done in 

**Related Issue(s)**



None

**Author(s)**

Stan Moore (SNL) and Claude Code (Opus 4.8)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

I used Claude Code (Opus 4.8)

**Backward Compatibility**

Yes

**Implementation Notes**

Description from Claude:

## Summary

Fixes several issues in the automatic parameter selection (`g_ewald`, k-vectors) of the `ewald/disp` dispersion solver, and hardens the shared `g_ewald` Newton-Raphson solvers across KSPACE against an overshoot failure. All changes are no-ops for the common, already-converging case — verified that the auto case is byte-identical before/after — and only change behavior where the old code was inaccurate or failed.

Found while validating dispersion long-range corrections for a planar liquid–vapor interface, where `ewald/disp`'s surface tension drifted with cutoff in a way the error estimator did not predict.

## Changes (one per commit)

**1. `ewald/disp` honors `gewald/disp` for the dispersion reciprocal sum.**
The single reciprocal k-sum and the self-energy used `g_ewald` for *all* interaction types; `g_ewald_6` was used only by the real-space pair (via `extract`) and as a copy target. So `kspace_modify gewald/disp <v>` pinned only the pair's dispersion damping while the reciprocal/self kept the auto-estimated `g_ewald` — silently desyncing the Ewald split (total energy/pressure became `g`-dependent, i.e. wrong, with no error). The dispersion (1/r⁶) coefficients, self-energy, and the LJ k-error term now use `g_ewald_6`; Coulomb/dipole still use `g_ewald`. When `gewald/disp` is unset, `g_ewald_6 == g_ewald` and results are unchanged.

**2. Real-space RMS force error estimate + Newton-solver robustness.**
- The LJ branch of the real-space error estimate under-predicted the true RMS force error by ~3–6× (bulk) and ~10× (interfaces). Replaced with the corrected Kolafa–Perram / Isele-Holder Eq. 20. *Note:* the published Eq. 20 [Isele-Holder *et al.*, J. Chem. Phys. **137**, 174107 (2012)] has a dimensional typo in the denominator (`sqrt(N) V Rc`); the dimensionally-consistent form is `sqrt(N V Rc)`, which matches measured bulk errors to ~Kolafa–Perram accuracy. (`pppm/disp` already used the correct form.)
- `NewtonSolve`: the raw step could overshoot the splitting parameter to `g ≤ 0` at large cutoff, abort, and silently fall back to a crude estimate. Added a damped/backtracking step (halve until `g > 0`) + zero/NaN-derivative guard. Converges for all cutoffs now.

**3. Reciprocal-space RMS force error estimate.**
The LJ branch over-predicted the reciprocal force error by ~300× (wrong `km`-dependence — it *grew* like `(π km/g/prd + 1)` instead of decaying like `1/√km` — and was missing a `1/√V`), so `ewald/disp` used far more k-vectors than needed. Re-derived (Deserno–Holm extended to r⁻⁶, large-h asymptotic `B(h) ~ 24 g⁵ e^{−h²/4g²}/h²`); the new estimate matches measured reciprocal errors with the correct scaling. End-to-end on bulk: the delivered force accuracy now tracks the request (err/acc ≈ 0.7–1.2 over 1e-3…1e-5) while using ~20% fewer k-vectors.

**4. Harden the remaining `g_ewald` Newton solvers.**
The same un-damped step is shared by `EwaldDipole::NewtonSolve`, `PPPMDipole::find_gewald_dipole`, `PPPM::adjust_gewald`, and `PPPMDisp::adjust_gewald_6` (Kokkos/electrode/spin variants inherit these). Applied the same damping + derivative guard. No-op for the common case (Coulomb `pppm` picks an identical `g_ewald`/grid before/after).

A tree-wide survey of Newton solvers found the others already robust or not of this kind: SRD (rtsafe bracket+bisection) and ASPHERE superellipsoid (regularized fallback) are guarded; PTM polar decomposition and the INTEL SIMD rsqrt refinement are not domain-constrained root-finders. GRANULAR's MDR solver has an unbounded `sqrt`-domain Newton that may merit a domain-specific bound — left for its maintainers.

## Validation
- Auto-`g` (no override): byte-identical energy/forces/pressure before vs after, across accuracy and cutoff.
- Corrected real- and reciprocal-space estimates verified against directly-measured RMS force errors on a bulk LJ liquid (Kolafa–Perram-level agreement) vs the old ~3–300× discrepancies.
- `gewald/disp` override now yields a `g`-independent total matching the `gewald` knob and `pppm/disp`.
- Newton solvers converge for all tested cutoffs; the damping change does not alter any previously-converging case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4168f60526e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.