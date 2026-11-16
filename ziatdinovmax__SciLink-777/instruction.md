# Issue

**Title:** XRD identification: the match overlay is drawn where its score was computed

## Description

## Summary

An XRD phase identification could report a good match while its own figure contradicted it. The match was scored after the reference pattern had been shifted and stretched to line up with the data, but the overlay was drawn from the unshifted reference. Every overlay peak then sat beside its data peak, by an offset that grew with angle. A large stretch that the match needed was never mentioned, and the explanation the agent wrote for the figure could rationalise it away.

This PR makes the figure and the number agree, and states the stretch:

1. **The overlay is drawn where the score was computed.** Every scorer now reports the shift and scale it fitted. A new tool applies exactly that to the reference pattern, and its legend says so, e.g. "Simulated TiO2 (match overlay; lattice scale ×1.022)". A second new tool draws the figure, with labelled axes and the data's own background.
2. **A large stretch is a caveat on the result.** A fitted lattice scale more than 1 % from 1 against a measured reference cell (3 % against a computed one) is reported with the result, and what it can mean is spelled out: a computed reference cell, a different or substituted phase, or a strained lattice.
3. **A measured reference cell is preferred.** When the same structure comes from both COD (measured cells) and Materials Project (computed cells, typically 1–3 % too large), the measured one is kept. The computed entry's id and stability ranking are carried over.

In live runs on a pattern whose reference cell is 2 % off, `main` drew the overlay off every peak and reported "high confidence" with no caveat. This branch draws it on the peaks, states the ×1.022 scale and reports it as a caveat.

---

## Technical details

### Changes
- **`xrd/overlay.py` (new):**
  - `registration_record` describes the transform a scorer applied: `2θ' = two_theta_scale · bragg(2θ, lattice_scale) + zero_shift`. `apply_registration` applies it.
  - `register_overlay(simulated, match_result, phase=None, formula='', reference_cell=None)` is a `TOOL_SPEC`. It moves the full simulated pattern by the scorer's registration and returns the label and `warnings`. For the plotted match, it prints the warning as a `TOOL_WARNINGS_JSON:` marker, worded as the scorer worded it.
  - A multiphase phase is resolved by index, then id, then a formula only one active phase has. An ambiguous formula, or no `phase` with two or more active phases, raises.
  - `plot_match_overlay(ax, x, y, overlays, fwhm='auto', background='auto', difference_ax=None)` is a `TOOL_SPEC`.
    - It broadens to the data's peak width and draws on a smoothed running-low-percentile background.
    - It uses a log y axis only when the peaks above the noise span more than 1.5 decades.
    - It draws a sum curve only for two or more phases, adds an optional "Data − overlay" panel, and always labels 2θ and the tick labels.
  - `lattice_scale_warnings`: bands of 1 % (measured or unknown cell) and 3 % (computed). The reference kind is read from the candidate's source; a local CIF is unknown.
- **Scorers:**
  - `score_xrd_match_robust`: hanawalt is a Bragg lattice scale; MIP is a linear 2θ scale plus a zero shift.
  - `score_xrd_match_fast`: a linear 2θ scale plus a shift.
  - `score_xrd_match_multiphase`: a per-phase Bragg scale, then the shared 2θ scale and shift.

  All of them return `registration` and `warnings` (multiphase per active phase) and take `reference_cell`. Matching and scoring are unchanged.
- **`search_structures._dedupe`:** one structure is keyed once whatever each backend writes: the reduced formula and the space-group number, falling back to the strings. COD ranks ahead of MP, which still ranks ahead of local CIFs. The entry kept records `also_in` (another database's id), an energy above hull the other source reported, and the better rank score.
- **Curve agent (`curve_fitting_controllers.py`):** `_parse_script_markers` collects `TOOL_WARNINGS_JSON:` lines. `_reported_warnings` merges them with a `warnings` list in the script's `FIT_RESULTS_JSON` (deduplicated, at most five, clipped), and they become the unit's `caveats`. A script that reports none is unchanged.
- **Synthesis prompt:** the caveats heading now covers both the fit's checks and its analysis tools.
- **`xrd.md`:** draw each overlay through `register_overlay` and the figure with `plot_match_overlay`, and pass each candidate's source as `reference_cell`.
- **CLAUDE.md:** the tool-warnings convention, under the skill subsystem.

### Tests
`tests/test_xrd_overlay_registration.py` (11 tests, generic data):
- **Registered overlays land on the data:** for hanawalt, MIP, fast (the shift's sign in both directions) and multiphase, each overlay lands within 0.2° of the data peaks; unregistered, it was more than 1° off. Every peak a robust scorer matched is drawn exactly at its `sim_pos`.
- **Polymorphs:** two active phases with one formula are named by id; the shared formula raises. A term that rounds to nothing is left out of the label.
- **Warning bands:** 1.022 fires against a measured cell, 1.003 doesn't, 1.022 doesn't against a computed one and 1.035 does.
- **Caveat flow:** the marker reaches the unit's caveats through the real `_fit_single_spectrum`, merged with a copy of the same warning that the script reports itself.
- **Dedup:** each backend's own notation; the measured cell wins in either order and keeps the computed one's id, stability and place.
- **The figure:** labelled axes and ticks, one line for one phase, drawn on the data's baseline.
- **Inventory:** both tools are registered.

**Full suite vs `main` (bc059709):** no failure on this branch that `main` doesn't have, by test id.

### Live A/B (Opus 4.8 on Bedrock, frozen copies of `main` and the branch)

| case | `main` | this branch |
|---|---|---|
| synthetic pattern, reference cell 2 % off, 4 runs per arm | matched that cell in 3 runs: overlay off every peak, "high confidence", no caveat | matched it in 2 runs: overlay on the peaks, ×1.022 in the legend, caveat. Picked a closer cell (×0.996) in 2 runs: no caveat |
| five reference mineral patterns | identified | same identifications |
| one reference mineral pattern | wrong phase, asserted | wrong phase, caveat fired (×0.984), and the claim calls the match unconfirmed |

- **Final code:** 4 more runs. Overlays drawn on the data's background, caveats as above.
- **Tool use:** every branch script drew through both tools.
- **Dedup, live with the real MP and COD backends:** before, MP's DFT cell of a structure was kept and COD's identical structure listed separately. Now COD's cell takes MP's place, with `also_in` naming MP's id.
- **After the review:** 3 more runs on the final code, all identified correctly, both tools used.

### Not covered
- **The misidentified pattern:** a separate identification gap, on `main` too.
- **The caveat flow is curve-agent only:** image and hyperspectral scripts don't lift `TOOL_WARNINGS_JSON` yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc059709d7a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.