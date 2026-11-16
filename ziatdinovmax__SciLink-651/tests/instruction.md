# Issue

**Title:** fft_defect: sign-aware deficit gate so vacancies aren't under-detected

## Description

.

## Summary

When `fft_defect` looks for point defects in a lattice, it flags both **adsorbates/extra units** (a bump *above* the lattice) and **vacancies/missing units** (a dip). It was holding both to the *same* height bar. But an adsorbate can pile up with no ceiling, while a vacancy can only drop as far as the pore/substrate floor — a shallower signal. So the single bar was about right for adsorbates but too strict for vacancies, and vacancies were systematically **missed**.

Live example (AFM-height protein honeycomb): 67 adsorbates vs only 10 vacancies, even though vacancies were visibly present — the residual map ran to +13.8σ on the adsorbate side but only −8.7σ on the vacancy side.

This adds a **separate, optional gate for the vacancy side** so it can be set lower when needed, without changing anything by default.

<!-- Technical details -->
### Technical details

- New optional params `deficit_null_percentile` / `deficit_k_min`. Both default `None`, which reuses the symmetric excess values, so the gate is **byte-identical unless set** (`(r>thr)|(-r>thr) ≡ |r|>thr`). The anomaly mask becomes sign-aware: excess must clear `threshold`, deficit must clear `threshold_deficit`; `threshold_deficit` is now reported.
- Per-candidate `strength_sigma` and the null calibration are unchanged; only the gate is split.
- TOOL_SPEC documents the knobs with which-way-to-tune plus the physical rationale (deficits are bounded → a lower deficit gate is justified when missing units are undercounted, e.g. AFM-height honeycombs; only the deficit side moves, so excess counts are unaffected).
- Test: default is a symmetric no-op; lowering the deficit gate recovers more deficits while the excess count is unchanged. Empirically on the live honeycomb, lowering the deficit gate tripled vacancy detection (4→12 raw) with excess fixed at 22.
- No-op verified for both callers: `tests/test_fft_defect.py` (18) and `tests/test_atomic_stem_lattice_tools.py` (20) all pass unchanged.

Defaults are untouched; a caller (guided by skill) opts in when vacancies are visibly undercounted. A fully-automatic amplitude-relative deficit gate (scale the deficit bar by the local lattice modulation depth) is a possible follow-up but is more invasive and left out here to keep this change a safe no-op by default.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73a80a3b81fa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.