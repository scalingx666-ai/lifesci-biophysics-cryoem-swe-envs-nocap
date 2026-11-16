# Issue

**Title:** A pin on a non-target component is a caveat, not a degenerate fit

## Description

## Summary

.

**What went wrong.** In a curve-fitting series, one parameter stuck at its bound on *any* component of the anchor's fit withheld the whole regime. On real multi-component spectra the pinned parameter was consistently a secondary component (a broad water hump, a weak side band, a background term) while the band the study asked about fitted well. With #741, every withheld unit also leaves the optimization.

**What changes.**
- **The script declares its targets.** A fitting script now declares `targets`: the components whose parameters answer the plan's `parameters_to_extract`. Background, baseline and overlap-only components are not targets.
- **A pin on a target still withholds,** exactly as before. A pin on a centre makes that component's area wrong too, so it counts at the component level.
- **A pin elsewhere is a caveat.** A pin on any other component:
  - is recorded on the unit (`secondary_pins`, plus a caveat line);
  - has its pinned value reported as no value, since a bound is not a measurement;
  - withholds neither the unit nor, through its anchor, the regime;
  - is not sent through the "relax the bound and refit" retry, which on the real runs never cleared such a pin.
- **No declaration means today's rule.** A fit that declares no targets, or names none of its own components (a typo must not switch the check off), is judged exactly as before.

**Real data (Bedrock `us.anthropic.claude-opus-4-8`):**
- **Ibuprofen sodium dihydrate, in-situ FTIR (10 spectra, 35–120 °C):**
  - the hydrated anchor's six fingerprint-band pins (`fp_1265`, `fp_1210`, `fp_848`, …) became caveats;
  - a pin on the water bend, which the script declared a target, still withheld that regime, which is the right outcome for a band the objective asked about;
  - a refit that wrote fresh code declared its own (broader) targets, and its pins were judged by them.
- **Dy(NO₃)₃ aqueous Raman series (9 spectra, the #729 data):** the model declared `targets` (ν1, ν4, the water bend, a low-frequency band) and left `baseline` out. Nothing pinned this time, so all 9 units verified.

**For review.** The rule helps only as much as the script declares targets narrowly. On the ibuprofen data the model also listed fingerprint bands, which errs on the safe side (more withholding, never less). The design question #726 raised, per-claim evidence, is still open; this is the interim step #742 proposed.

---

## Technical details

- **`curve_fitting_tools.split_pins_by_targets(pins, targets, parameters)`** returns `(target_pins, secondary_pins)`. It falls back to all-target when `targets` is undeclared, empty, not a list, or names no fitted component.
- **`_fit_single_spectrum`:**
  - the attempt loop's pinned-at-bound retry and the held-follower log consider target pins only;
  - after the loop, secondary pins null their parameter values in `parameters`, are logged as a caveat, and are recorded as `result["secondary_pins"]`, `fit_quality["secondary_pins"]` and `result["caveats"]`;
  - `pinned_at_bound` (which the verdict, the follower verdict and `_detect_outliers` read) holds target pins only;
  - `result["targets"]` is recorded.
- **Contract:** `FITTING_SCRIPT_INSTRUCTIONS` gains `"targets": [...]` in the results JSON; `FITTING_SCRIPT_CORRECTION_INSTRUCTIONS` says to keep it unchanged. A test captures both rendered prompts from the real prompt builders.
- **CLAUDE.md:** the series paragraph.

**Tests** (`tests/test_secondary_pins.py`, 6, through the real series driver and `_fit_single_spectrum`):
- the split, and its fallbacks;
- a follower pinned on a secondary component verifies with the caveat and a null value, is not relaxed and is not flagged;
- a target pin still withholds;
- no declaration is judged as before;
- an approved fit with only secondary pins is verified, and a target pin on the anchor still withholds the regime;
- the contract is in the generation and correction prompts.

Full suite against a `main` worktree: no branch-only failures. The curve QC goldens already fail on `main` (an assertion before any prompt comparison), so the prompt change is not golden-checked.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff620304bd3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.