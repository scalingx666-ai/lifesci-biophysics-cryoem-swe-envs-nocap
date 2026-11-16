# Issue

**Title:** Series regimes: judge membership from the data when the series axis is not a physical ordering

## Description

## Summary

When the curve-fitting agent analyzes a **series** of spectra, it first scouts a handful of them, runs an unsupervised change detection over the whole series, and asks the planner to split the series into regimes. That workflow assumes the series variable is a physical axis (temperature, time, concentration) along which regimes are contiguous. When the series variable is only a label (file order, sample id), regimes can interleave, and a spectrum that was never scouted could be filed into the wrong regime by its position alone. In a live run, a run with a large step was analysed as a "no-step control" for exactly that reason.

This PR lets the data decide in the cases where position cannot:

- The change detection now reports whether the series axis actually orders the spectra into contiguous regimes (an **axis-coherence** measure) and gives a per-spectrum score.
- When the axis is not coherent, every spectrum is scouted (up to 16), the planner is told to assign membership per spectrum and that regimes may be non-contiguous, and a spectrum that clearly belongs elsewhere is moved (and logged).
- Spectra the planner leaves out of every regime now join their nearest neighbour along the axis (or the nearest regime in score space on an incoherent axis) instead of always the first regime.
- On a normal monotone series a boundary spectrum between two scouts is moved only when it is an unambiguous misfit; small transition bins and gradual ramps are left exactly as planned.

Conventional series behave as before: the repository's live monotone-series test passes unchanged (same scouts, same change point, same three-regime plan), and all existing series tests are green.

---

## Technical details

- `scilink/skills/_shared/series_reduction.py`: `reduce_curves` adds `axis_coherence` (`n_large_steps`: PC1 steps > 30 % of the score range along the sorted axis; `n_reversals`: direction reversals among them; `coherent = n_reversals < 2`), `scores_by_index` (PC1 score per curve in input order) and `controls_by_index`.
- `SeriesScoutController.execute` runs the reduction before scouting; `_select_scout_indices(n, scout_all=...)` returns every index (cap `_SCOUT_ALL_MAX = 16`) on an incoherent axis and the previous stride otherwise. Log line explains why.
- Planning prompt: on an incoherent axis the "place regime boundaries" sentence is replaced by an "AXIS NOT COHERENT" block with the per-index PC1 scores and permission for non-contiguous `spectrum_indices`; coherent series get the previous text verbatim.
- `_correct_regime_membership(regimes, reduction)` after `_extract_series_plan`: incoherent axis — move a spectrum whose PC1 score is farther than 25 % of the range from its own regime's median than from another's; coherent axis — stricter (40 %), the score must lie inside the target regime's observed range (padded 10 %), the source regime needs ≥ 3 members, and a regime is never emptied. Moves are logged and recorded as `series_plan["membership_corrections"]`.
- `_assign_missing_indices`: nearest assigned neighbour along the axis (ties resolved by PC1 score when available); nearest regime median in score space on an incoherent axis; first-regime fallback only when nothing is assigned.
- Tests: `tests/test_series_regime_membership.py` (7 cases: coherence flags, input-order scores, scout-all gating, membership correction on interleaved / sharp / gradual / small-bin series, missing-index placement) plus the existing `test_series_reduction.py`, `test_scout_series_reduction.py`, `test_series_plan_revision.py` (17/17), `test_best_of_n_planning.py`. Live: `tests/test_scout_reduction_live.py` passes 5/5 on the final code (change point 405 K, 7 of 16 scouted, boundary at index 10/11); an interleaved nine-trace series (controls interleaved with two kinds of stepped runs) is grouped correctly with the omitted spectra placed by neighbour.

### Added after the first live rounds (same branch)

- **Cluster consensus.** When the PC1 scores form well-separated 1-D clusters (gaps > 30 % of the range) and there are exactly as many clusters as planned regimes, membership is read off the clusters; each regime keeps its name and model, matched to the cluster whose centre is closest to the regime's current median score (overlap breaks ties). Repairs a partly scrambled plan where per-spectrum medians are meaningless; no-op for a gradual series or when counts differ.
- **Split-and-merge on an incoherent axis.** A regime must not straddle separated clusters (e.g. opposite-signed responses lumped into one "step" regime, whose median then sits between clusters): such a regime is split per cluster (same model, name suffixed), regimes sharing one cluster are merged (one cluster → one regime), then the consensus places strays. Never applied on a coherent axis.
- **Bounded iteration** of the per-spectrum correction (moving a spectrum changes both medians); a **≥ 3 members** rule for source regimes on a coherent axis; a regime is never emptied; the target regime's score range is padded 10 %.
- **Variance study on the conventional live test** (`tests/test_scout_reduction_live.py`, sharp switch at 405 K): final code 3 × 5/5 and 1 × 3/5; untouched `main` 1 × 5/5 and 1 × 3/5. The 3/5 outcome is the pre-existing "plan validation revises to a single regime" model path in both trees, not a regression.
- **Interleaved live check** (nine traces, three condition groups interleaved in file order, on the integrated tree): correct three-group plan in every run, planner slips corrected from the data, final numbers matching an independent deterministic derivation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `faf3423aea37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.