# Issue

**Title:** What a curve fit reports is checked against the fit and the data

## Description

## Summary

A curve fit could look right, pass every gate and be verified, and still hand wrong numbers downstream. Two ways this happened in live runs:

- **A script read the wrong column.** A locked recipe picked its y column by matching the first spectrum's own value range. On the next spectra nothing matched, it fell back to the x column, and every number it reported was a statistic of x. Its self-reported R² still passed, so those units were verified and their claims posted as verified.
- **A reported number did not belong to the fit.** Examples: a band centre reported outside the measured axis, a width wider than the whole axis, or an uncertainty taken from a fit that was itself flagged as degenerate.

This PR closes both:

1. **Where x and y sit in the data file is stated, and so is the rule.** The prompt that writes a script, the prompt that refines it and the plan check carry the rule: never pick a column or a window by matching a value read off one spectrum. The generation prompt and the two repairs that never see it (a saved-fit mismatch, a script-bank adaptation) also say where x and y sit.
2. **The saved fit must be a fit of the data.**
   - A fit that follows the x axis is sent back for correction. If it still does, the unit fails and can be refit. It is never verified on its own reported R².
   - A replay of a locked recipe that does not follow its data even up to a constant offset, where the recipe's anchor did, is **withheld**: the recipe is kept, nothing is repaired or refit, and the reason is on the record. This also applies when a recipe from an earlier run is reused.
3. **Numbers the data cannot support are reported as "no value", with the reason.** That covers a band centred outside the axis ("not measured"), a width wider than the axis, and the uncertainties of a degenerate fit. A fit in which nothing that was asked for was measured is withheld, not failed, whether it was fitted fresh, replayed in a series or reused.

In a live test, replaying a recipe that picks its column by value on its followers reports statistics of x, verified, on `main`. On this branch the same replay is caught and repaired, and its values match an independently fitted series.

---

## Technical details

### Measured before choosing the rules (4,595 saved real units)
- **Saved fit vs data:**
  - "Follows the x axis" (R² < 0 against y and R² > 0.5 against x) fires on 6 units, all fits of the wrong column.
  - "Replay R² < −1 up to a constant, where the anchor's ≥ 0" fires on 2 of 259 followers with a known anchor, both fits of the wrong column.
  - A saved fit below a flat line on its own is common and healthy: peaks-only fits saved without their baseline, windowed models evaluated outside their window, a level fitted to a flat control. So it is not a rule.
- **Why up to a constant:** a peaks-only recipe whose background rises along a series gives plain R² values far below zero on healthy followers, but an offset-free R² of about 1.
- **Names:** a band is read only when a bare position name (`center`/`centre`/`position`/`pos`/`x0`/`cen`/`loc`) and a bare width name (`sigma`/`gamma`/`width`/`fwhm`/`hwhm`) sit on one component. These are not read:
  - a unit suffix (a position in another unit than the axis);
  - a derived quantity (`center_separation`);
  - `mu`, which is as often a level's mean as a position.

  With that reading, the centre check fires 7 times and the width check 6 times, and the plots checked confirm them as real.
- **Declined:** holding a series' claims provisional on fired follower `regime_checks`. They fire on 205 of 259 followers, healthy series included.

### Why the replay rule withholds and never repairs
A recipe fitted over a window with a local linear baseline saves that line extended across the whole axis. When the local slope changes along a series, a healthy follower can score below −1 up to a constant. Comparing up to a straight-line baseline instead was measured and declined: it clears both real wrong-column replays in the corpus (their wrong fit and their data are both close to a ramp in x).

Live A/B, with four options on frozen copies (repair up to a constant, repair up to a line, withhold-only, x-axis rule only):
- **Real data:** 12 real series across five techniques, 3 reuses of a recipe across a phase change, and the earlier series: 212 replayed units in 56 runs. The replay rule fires on 4 of them:
  - the two wrong-column replays, which the x-axis rule catches first when run live;
  - two followers of one series whose anchor recipe was not approved, so every unit was already not verified. One replay diverged. The other is the windowed shape above: the fit follows its window, and its local baseline runs on as a straight line past it.

  Under the repair behaviour, both would have rewritten the locked recipe; withheld, nothing changes. No other unit comes near the cut-off (lowest offset-free R² +0.48).
- **The wrong-column replay:** in all 8 runs the x-axis rule caught it first and the repair reached the right values, under every option.

The replay rule has no live catch of its own, and its one false-positive shape has now been seen on real data. So it only withholds a certificate (#711); the x-axis rule keeps the repair.

### Changes
- **`instruct.py`:**
  - `FITTING_SCRIPT_INSTRUCTIONS` gets a `Layout:` line and the rule in requirement 1. Deriving a window from the data at run time stays allowed.
  - The correction I/O contract and the conformance execution contract carry the rule.
- **`curve_fitting_controllers.py`:**
  - `_data_layout(curve_data)` follows the same rule as `_extract_xy`; an unrecognised shape names no layout.
  - `_saved_fit_mismatch(curve_data, fit)`: the x-axis rule only. A mismatch goes to the correction with `data.npy layout: …`. If it persists, the unit fails with `kind: saved_fit_mismatch`. A strict or verbatim replay makes no model call.
  - `_replay_misfit(curve_data, fit, anchor_r2)`: the offset-free replay rule (`REPLAY_R2_FLOOR = -1`). It records `fit_quality["replay_misfit"]` (a list, so no `features.csv` column), a caveat, and a `replay_misfit` flag that is not a refit reason. The recipe is kept. A follower whose gate failed too is refit as on `main`, and stays withheld if no refit replaces it.
  - A deliberate exception to #726: only the x-axis rule repairs a follower's locked recipe, because that recipe read the wrong column of the unit's data.
  - Before each run an earlier `fit.npy` is set aside (`fit.npy.previous`), so only the fit this run wrote is read, in the loop and after it. A timestamp cannot tell a rewrite within one tick of a coarse filesystem clock.
  - `result["saved_fit_r2"]` (offset-free) goes to followers through `anchor_saved_r2_by_regime`, and onto the recipe and `certification_reference.saved_fit_r2`. Each reuse candidate is held to its own recipe's value, except a script-bank cold start.
  - `_reported_outside_axis` joins #764's `not_measured`. `fit_quality["no_target_measured"]` is a list. `_is_band` reads `cen`/`loc` like `_band_names`.
  - `_wider_than_axis` and `_withhold_uncertainties` produce `withheld_values` (a caveat, a non-refit flag in `CAVEAT_FLAGS`). It covers:
    - a width wider than the axis;
    - the reported uncertainties of a degenerate fit;
    - a secondary pin's component uncertainties.
  - An uncertainty is read by structure first (`X_err…` / `X_ci95` beside a reported `X`, a bare `err` beside a `value`), then by a word of its name. A fit metric (`rms_error`, `fit_error`, `mean_abs_error`) and a `_std` (as often a measured scatter) are never read. A number or a numeric interval is withheld, never a flag.
  - A unit keeps one flag. Values withheld that the flag does not name are added to its text ("Also reported as no value — …").
  - The bank-adapt `output_contract` asks for x and y from their fixed positions, never a column picked by value (its own rules re-derive windows for the new data), and its context carries the layout.
- **`_verification_record.py`:** `fit_check_withheld(item)` (nothing measured, or a replay misfit) is read by every branch that can verify a curve unit: its own gate, a follower's recipe, the replay-gate branch of a reuse (single run and series), and the legacy reconstruction.
- **`_replay.py`:** `curve_certification_reference(..., saved_fit_r2=None)`.
- **`orchestrator_tools.py`:** the planning ingestion names the `withheld_values` reason.
- **CLAUDE.md:** one paragraph in the series-analysis section.

### Tests
- **New:** 27 tests in `tests/test_reported_fit_checks.py`, through the real paths:
  - **Repairs and prompts:** the real correction prompt in both layouts; the bank-adapt prompt.
  - **Saved-fit rules:** a strict replay making no model call; a fit rewritten within one clock tick; a stale `fit.npy` in and after the loop; the non-firing cases, with the anchor-below-zero exemption.
  - **Replay misfit:** a follower through the series driver (kept, one run, flagged, not refit, withheld); `qc_try_reuse` replaying a board copy (kept verbatim, no model call, withheld), and with no reference or a bank cold start (nothing recorded); a rising-background series staying on `main`'s path.
  - **Nothing measured:** a reuse; a series anchor that replayed a recipe, stamp and legacy reconstruction alike; a follower through the series driver.
  - **Reported values:** centre, width and uncertainty withholding, by structure and by word (fit metrics, scatters and flags kept); both band readers agreeing; one flag per unit; and a guard that a fit where nothing fires is reported exactly as printed.
- **Full suite vs `main`:** no failure on this branch that `main` doesn't have, by test id (`main` has one more, a timing-sensitive Ctrl-C test).

### Live checks on the final code
- **The wrong-column replay, twice:** caught by the x-axis rule, repaired, every follower at the independently fitted values, verified.
- **A healthy series and two reuses across a phase change:** all units verified; no saved-fit check fired.
- **Bank edit-adapt** (the prompt changed): the existing live harness against an isolated bank adapts with a small diff (9 % of lines) and records its provenance, as on `main`. On every version the harness's log-line check fails, because it looks for a message that is no longer logged. Whether the record's success count is raised varies from run to run, `main` included.

### Not covered
- **No anchor reference:** a bare recipe `.py`, a unit script named inside a series run, and a run recorded before this PR carry no anchor saved-fit R², so only the x-axis rule protects them.
- **A narrow miss of the offset-free rule:** a wrong-column fit that matches the data's shape up to an offset is not caught by it (a switch outside the central part of the axis with a similar step). The case this PR was opened for is still caught.
- **Outlier pass unchanged:** `_detect_outliers` still scores the self-reported R² and skips units with no metric.
- **Two wordings for one finding:** a centre held at the axis end by a bound (#761) reads as a degenerate fit, while a centre reported outside the axis reads as not measured. The verdicts agree.
- **A multi-regime reuse** keeps a recipe the gate passed even when it measured nothing or does not fit; it is withheld, and the next regime's recipe is not tried (`select_recipe` unchanged).
- **Check 4 of the issue:** per-component curves saved beside `fit.npy` are a contract change left for later.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce2516b63f07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.