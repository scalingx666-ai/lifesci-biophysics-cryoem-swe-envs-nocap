# Issue

**Title:** Pure lineshape limits are not degenerate fits; a band beyond the axis is not measured; the saved fit is in the data's space

## Description

## What this changes

A curve fit is withheld as "degenerate" when a fitted parameter ends at the edge of the range the script allowed. That rule is sound: a value stuck at its limit was constrained, not measured. But it also withheld fits for values that are not failures at all:

- **A pure lineshape.** A Voigt line is part Gaussian, part Lorentzian. When the data say a band is purely Lorentzian, the Gaussian width goes to its floor, and the fit is not broken. The same holds for a mixing fraction at exactly 0 or 1. The rule recognised such fractions only by a few names (`eta`, `fraction`, …), so `fL` and the like slipped through.
- **A band peaking outside the measured range.** At the edge of a spectrum, a band whose maximum lies beyond the axis can only be fitted with its centre held at the axis end (or at a bound the script widened past it). None of that band's values can be measured, but the rest of the fit is fine. Fitting scripts declare every band they report as a target, so on `main` one such band withholds the whole fit.

Now, in curve fits only:
- Both pure-lineshape cases are no longer called degenerate. The width case applies only to the Gaussian/Lorentzian pair of one line.
- A band whose centre is held at an end of the axis, or at a bound the script widened past it, is reported as **not measured**, target or not. All its values are empty (centre, width, area and their errors), since half a profile measures none of them. Any other stuck value on that band goes with it. The run carries a caveat naming the band, and the other bands stand. In a series, the spectrum carries a `not_measured` flag that never triggers a refit and never counts as a failing frame.
- A fit with **no** target left measured stays withheld: nothing the plan asked for was measured. The targets are the declared names that are fitted components, else every fitted band, since a background measures nothing the plan asked for.
- The written interpretation, of a single spectrum or a series, is told why a value is empty, and that any value derived from the band elsewhere (a ratio, a relative intensity normalised to it, a total) is not a measurement either. Such values are named freely by each script, so no rule can find them.
- When planning ingests the feature table and skips a unit for its empty value, the warning names the unit's flag. A value left empty on purpose (not measured, a secondary pin) is said to be so, instead of suggesting the quantity sits under another column. A unit with any other flag (an outlier, a low score) keeps that hint, since its value may exist under another name.
- Real failures still are: a single width collapsing to zero, two different peaks' widths, a capped amplitude, a position on a normalised axis, a real floor (an instrument resolution).

The hyperspectral scalar check shares the rule and is unchanged.

Separately, the fitting script's contract now says the saved fitted curve (`fit.npy`) must be in the **same space as the data, baseline included**. Some scripts saved the model without the baseline they had subtracted. That silently corrupted the residual diagnostics the verifier reads. The recomputed R² only ever raises the self-reported one, so it was not affected.

This is the corrective part of #761. That issue's heuristic parts (measured-feature windows, a longer repair loop) were measured and dropped; see its closing comment.

---

## Technical details

- `curve_fitting_tools.validate_bound_pinning(..., lineshape_limits=False)`: the exemptions apply only with the flag, which the two curve call sites pass; the hyperspectral `_check_scalar` does not, so it keeps `main`'s rule.
  - a parameter bounded exactly `[0, 1]` that is not an amplitude, a position or a width (by word) is a fraction, exempt at either end;
  - a width at its lower bound is exempt only when its Gaussian/Lorentzian partner of the same line carries the line. The partner is found by name: one name has a Gaussian word (`sigma`, `g`, `gauss`), the other a Lorentzian word (`gamma`, `l`, `lor`, `lorentz`), and the rest of the two names is equal (`sigma`/`gamma`, `p1_sigma`/`p1_gamma`, `fwhm_g`/`fwhm_l`). The partner must be clear of its own floor, at least 10× this width, with this floor non-negative and no more than 5 % of it (a value at a negative bound is a skewness or an offset, not a width at zero). The pairing is by name only, which the docstring says;
  - still pinned: a single width collapsing, two peaks' widths, the two sides of an asymmetric profile, a normalised amplitude or position at 1, a floor that is not small beside the carrying width, and a rate constant on a positivity floor.
- `curve_fitting_controllers._beyond_axis(pins, stats, bounds)`: a centre-like parameter pinned at a bound within 0.5 % of an end of the measured axis, or at a bound beyond it when the parameter's declared range reaches into the data (a position on the axis, not a relative shift, a log-space `mu` or an offset). Both the fit loop and the finalisation pass the fit's bounds.
  - In the fit loop it triggers no repair, since no bound can move the band's maximum into the data.
  - At the end, it is taken out of both the target pins and the secondary pins, into a new `not_measured` record on the result and its `fit_quality`, with a caveat (`describe_not_measured`). Every numeric value of the component is `None`, and the component's other pins are dropped.
  - Exception: if every target is beyond the axis, those pins stay in `pinned_at_bound`. The targets are `(declared ∩ fitted components) or fitted bands or fitted components`, where a band is a component with a position parameter (`_is_band`, the same tokens `_beyond_axis` uses).
  - `_synthesize_single_spectrum` adds the unit's `caveats` (not measured, secondary pins) as a "Fit caveats" section. The caveat and the series flag's recommendation share `NOT_MEASURED_NOTE`, which states the derived-value principle.
- `planning_agents/orchestrator_tools.py`: when rows are skipped for missing values, `_flag_reasons` reads the table's `unit` / `flag_reason`. The warning names each reason and the result carries `rows_skipped_reasons`. Only `not_measured` and `secondary_pin` (`_EMPTY_VALUE_REASONS`) are called empty on purpose and drop the "another column" hint; every other flag keeps it.
  - Series: `_detect_outliers` adds a `not_measured` flag (not in `REFIT_REASONS`). The series-context and series-mismatch readers that skipped `secondary_pin` now skip both, through `CAVEAT_FLAGS`.
- `instruct.py`: the main and correction prompts say `fit.npy` is in the same space as `data.npy`, baseline included.
- Tests (`tests/test_pin_limits.py`):
  - the limit exemptions and what still pins (the review's two-peak, split-profile, normalised-centre, real-floor and negative-bound skew cases);
  - the hyperspectral `_check_scalar` keeps failing a [0, 1] scalar at 1;
  - `_beyond_axis`, including a bound widened past the data; a relative shift, a log-space `mu` and an offset at a bound outside the axis are not beyond it, and through the fit loop they pin and are repaired;
  - through the real fit loop, with no targets, both bands declared, or only the other band declared: the edge band is not measured, has all values `None` and no repair, and the other band stands; its other stuck values don't withhold the fit; a fit whose only target is beyond the axis stays withheld, as does one whose declared targets include a name that is no fitted band, or name none, or are absent, alone or beside a background (a background declared as a target is the script's choice, and releases);
  - through a real series: the unit gets a `not_measured` flag, is not refit, and is verified;
  - the single-spectrum synthesis prompt carries the fit's caveats; the caveat and the series flag carry the derived-value sentence;
  - planning ingestion of a real feature table: a not-measured unit is skipped with its reason; a plain missing value and an outlier whose value sits under another column keep the "another column" hint; a table whose only skipped unit is not measured drops it;
  - the prompt contract (a string check: it pins the wording).

  The existing pinned-bound tests pass unchanged.
- CLAUDE.md: one clause in the series-analysis paragraph on pins.

**Measured.**
- **Offline:** on 134 fits an earlier A/B had withheld for pins, the new rules release 9, every one correctly.
- **Live A/B (Bedrock):** this branch against `main` on 79 cases (Raman, FTIR, NMR, EPR, EELS, XRD, XPS, DSC, TG, TRPL, plus runs with named targets). Every flagged case and a random sample were reviewed on their plots.

| | `main` | this PR |
|---|---|---|
| Recall of reference bands (strength-weighted) | 0.88 | 0.88 |
| Position error relative to band width (median) | 0.055 | 0.055 |
| Final fits with a strong band left out | 15 % | 10 % |
| Model calls per run | 19.1 | 16.8 |

- **What the rules changed directly:** 3 final verdicts, all pure-lineshape cases, all correct.
- **Every other difference between the arms** is run-to-run variance: the flagged cases involve no released pin.
- **Fewer fits saved in the wrong space:** the visible effect of the `fit.npy` sentence.

**After review** (live, Bedrock, frozen copies of each commit):
- **The narrowed exemptions** (19 cases, including bands at the axis end, declared targets, skewed lineshapes and controls): every pin released is a pure-lineshape width, paired with the other width of its own line. Every withheld fit is withheld for a band stuck inside the spectrum, which this PR does not touch. Reference-band recall 0.81 against `main`'s 0.86, and position error 1.9 against 2.0, are within run-to-run variance (per-case plots reviewed).
- **Not measured** (8 cases where a band peaked at the axis end in earlier runs): in each final fit with such a band, it is reported as not measured with all values empty, the caveat attached, and the other bands kept. The plots show the data rising into the axis end in each case. One run widened a bound past the axis; the rule now covers that too.
- **Re-review fixes** (the same 8 cases again; three were re-run after a provider outage spoiled them): two final fits report a band beyond the axis as not measured. In one, the fit is verified with the other bands kept. In both, the written interpretation explains the empty band (in one: "its parameters are null (not measured)"). Every withheld fit is withheld for a value stuck inside the spectrum. The plots are equal to `main` or better, except one control with a larger residual on a band flank, where nothing was released.
- **Follow-up notes** (2 cases): in Senarmontite the band at the axis start is not measured, and the fit is verified. The written interpretation reads: "an unmeasured edge component; nothing quantitative should be drawn from it". No value derived from it is quoted.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47a3de5f8a5c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.