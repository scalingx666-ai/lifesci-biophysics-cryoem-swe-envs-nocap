# Issue

**Title:** Treat a parameter pinned at its bound as a degenerate fit

## Description

.

## Summary

A locked fitting script that carried a numeric amplitude ceiling read off its first spectrum pinned at that ceiling on later, stronger spectra. The baseline absorbed the misfit, R² still passed the gate, and a wrong value was recorded as a success. Three changes, each general rather than tied to that trace:

- **Generated scripts use data-relative bounds** (an expression of the loaded data, or unbounded) and report the bounds they used.
- **A fit with a parameter at its bound is caught deterministically**, independent of R². The run relaxes exactly that bound once and refits; if it still pins, the fit is kept but stamped and flagged in the series so it cannot pass silently.
- **Locked-script reuse now engages for a single-regime series.** It was skipped for any planned series, which also kept the reuse path from being exercised; only a plan with more than one regime blocks it.

The check is deliberately narrow so it never calls a legitimate fit degenerate: a zero lower bound (a vanished component, a zero slope) and fraction-like parameters at 0 or 1 (a pseudo-Voigt at `eta = 1`) are not reported. Scripts that report no bounds behave exactly as before.

## Technical description

- `instruct.py`: `FITTING_SCRIPT_INSTRUCTIONS` step 3 states the data-relative-bounds principle and the results JSON gains `bounds` mirroring `parameters` (null for an open side); `FITTING_SCRIPT_CORRECTION_INSTRUCTIONS` gets a narrow exception permitting (and requiring) widening of the named bounds only.
- `curve_fitting_tools.validate_bound_pinning(parameters, bounds, rel_tol=0.01)` → `[{component, parameter, value, bound, side}]`; `describe_pinned`; `PINNED_BOUND_FIX`. Exemptions: lower bound of 0, fraction-like names (`_FRACTION_KEYS`), open-both-sides bounds.
- `_fit_single_spectrum`: after a successful run the parsed markers are checked; a pinned fit sets a `DEGENERATE FIT` error and continues to `_correct_script` (at most `MAX_PINNED_RETRIES = 1` per spectrum, within the existing attempt ladder). A fit that still pins carries `fit_quality.pinned_at_bound`, `pinned_at_bound`, `quality_warning` and a `script_errors` entry of kind `pinned_bound` (which the verifier context already consumes). `bounds` is passed through on the result.
- `_detect_outliers`: a successful result with `pinned_at_bound` is flagged with reason `pinned_at_bound` regardless of the gate metric; existing consumers already guard `r_squared`/`deviation_sigma` for None and the adaptive-refit selector keys on its own reason list, so nothing downstream changes for other reasons.
- `_reuse_blocked_by_regimes(state)`: true only when the series plan has more than one regime (the old guard tested `regime_configs`, which a single-regime plan also populates).
- Tests (`tests/test_pinned_bound_degeneracy.py`, 6): validator rules and exemptions; a pinned locked script corrected once and the relaxed fit kept with its reported bounds; a fit that still pins stamped after one relaxation; series flagging regardless of R²; the prompt contract; the regime guard. Full suite: identical failure set to main (56 failed / 34 errors pre-existing, 2607 passed).

### Live check (Bedrock)

A prior run whose script hardcodes `ub = [..., 0.45, ...]` for the product amplitude was reused on a four-spectrum series with true product amplitudes 0.2, 0.4, 0.8, 1.2.

- Control, script as-is with no agent: at a true amplitude of 0.5 it reports 0.450, the intercept inflates to 0.039 and R² is 0.990, so the old gate passed it.
- With the fix, reuse engaged (`reuse_validity: good`); spectra 2 and 3 pinned on attempt 1 (`product.amplitude = 0.45 at its upper bound 0.45`), the correction changed the ceiling to `2.0 * np.max(y)`, and the refits reported 0.799 and 1.199 with R² 0.9996 and 0.9998. Run twice with the final code, same outcome.
- A fresh anchor generated under the new prompt (earlier run) used `3.0 * yr` ceilings and reported its bounds.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da6cc29dfdca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.