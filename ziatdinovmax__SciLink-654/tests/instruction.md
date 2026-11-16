# Issue

**Title:** Hyperspectral series analysis: anchor + locked replay, regimes, aligned schema, parallel replays

## Description

## Summary

The hyperspectral agent can now analyse a **series** of datacubes (one cube per temperature, dose, time step, sample…) the way the image and curve agents already do, instead of refusing a list with "Batch processing not supported".

What you can do now:

- Pass a list of cubes (or a directory of cubes to `run_analysis`) with `series_metadata`. The first cube is analysed in full; its verified analysis script is **locked and replayed verbatim** on the others, so the extracted numbers (peak positions, intensity ratios, …) are directly comparable across the series.
- The agent **scouts the series first**: it overlays the mean spectra, runs change detection along your control variable, and splits the series into **regimes** when one script cannot serve every cube (e.g. an edge that shifts out of its fit window at high temperature, or samples that alternate between two states). Each regime gets its own locked script.
- Every cube reports the **same feature columns**. A cube that needs a fresh analysis (another regime's first cube, or a cube the locked script failed on) is re-analysed for the *same* targets and output names, and any remaining gap is reported, never a silent blank.
- Cubes whose features jump away from the series trend are **flagged** (not re-analysed — the anomaly may be the physics); cubes the locked script fails on are re-analysed within `max_series_refits`.
- You get a **trend dashboard** (`feature_trends.png`), a series-level interpretation with scientific claims, an HTML report, and `features.csv` with one row per cube and your control variable as a column.
- Replays are fast and can run **in parallel** (`series_workers=3`): about one minute per cube, overlapping with the rest of the run.

```python
agent = HyperspectralAnalysisAgent(output_dir="./eels_series")
result = agent.analyze(
    ["eels_300K.npy", "eels_350K.npy", "eels_400K.npy", "eels_450K.npy", "eels_500K.npy"],
    system_info={"technique": "EELS", "energy_range": {"start": 450, "end": 570, "units": "eV"}},
    series_metadata={"variable": "temperature", "values": [300, 350, 400, 450, 500], "unit": "K"},
    max_series_refits=1, series_workers=3,
)
```

Live, on a synthetic Ti L-edge temperature series with a planted +2.5 eV jump at 500 K, the agent placed the 500 K cube in its own regime, reported L3 positions of 458.5 / 458.9 / 459.3 / 459.7 / 462.7 eV under one column name, flagged the 500 K cube at ~13 sigma, and wrote claims describing the linear shift and the transition.

## Technical description

### Design

Series analysis is one shape across the three analysis agents (see CLAUDE.md, "Series analysis is one shape…"): anchor → locked recipe → verbatim replay → flagging → budgeted refit → trend codegen → series synthesis. The hyperspectral instantiation differs in mechanics, not shape:

- **One child agent per dataset** (`dataset_NNNN/`), because the single-cube pipeline is bound to one output directory. The "locked recipe" *is* the existing locked-script replay (#172) pointed at the regime anchor's `dynamic_analysis_records.json`. The parent owns the once-per-series decisions (skill selection, script banking, T=2 staging); replay children skip them (`_series_role`, `_skill_autoselect`).
- **Scout stage** (`scout_series`): every cube's spatially averaged spectrum → `series_reduction.reduce_curves` (change point, sharpness, axis coherence) + an overlay of representative cubes (evenly spaced, all when the axis is incoherent, plus the pair bracketing a sharp change point). **Regime planning** (`plan_series_regimes`): one LLM call with `HYPERSPECTRAL_SERIES_REGIME_SUPPLEMENT`; `extract_series_plan` validates (clamping, missing indices via the curve agent's `_assign_missing_indices`, empty regimes dropped); one retry on unparseable JSON (observed live: trailing comma), else one regime.
- **Locked-targets mode** (`analyze(locked_targets=…)`, `SelectRefinementTargetController`, `DecompositionController` bypass): targets and required output names fixed by the series' *schema source* (first dataset to lock), planning and decomposition skipped, code regenerated through the full ladder; the QC's required-outputs check enforces the names. The schema source's extra maps are requested by name. Used for regime anchors beyond the first and for adaptive refits. `complete_locked_schema` aliases residual drift (units suffix, diagnostic prefix) and records `locked_schema_gap`.
- **Deterministic replay gate** (`_replay_map_gate`): on a locked replay no LLM map review runs; a map passes on valid coverage (within the fit mask when scoped), a non-collapsed distribution, and — for required outputs — a median inside the anchor's [min, max] widened by one span (≥ 0.5 % of magnitude). The anchor's per-map stats travel as `replay_reference`. A rejection means the method broke on that dataset → refit. Replay children skip the synthesis critic/editor pair (`_light_synthesis`).
- **Flagging** (`detect_outliers`): `analysis_failed`; `unverified` (features committed from a salvaged attempt with no verified required output — excluded from statistics, refit-eligible after failures); `statistical_outlier` as a trend-aware score (deviation from a line through the other datasets vs. the control variable, scale floored at 5 % of the column range, only the dominant anomaly flagged), restricted to the primary outputs' mean/scalar columns, per regime when regimes interleave. `select_refit_candidates` honours `max_series_refits` (None unlimited, 0 none), failures first, one candidate per dataset.
- **Parallel replays** (`ReplayPool`, `replay_worker`): spawned-process pool (`SCILINK_HS_SERIES_POOL=thread` for tests), submission the moment a regime locks so replays overlap with later anchors, per-replay `replay.log`, parent's sandbox approval carried in the spec. Anchors and refits stay serial.
- **Contracts**: `series_analysis_results.json` uses the image agent's row shape, so `feature_table._image_series_rows` emits `features.csv` unchanged. The result dict mirrors the image/curve series result (`summary`, `individual_results`, `flagged_datasets`, `refit_summary`, `refit_skipped_by_budget`, `trend_analysis`, `feature_trends`, `series_features`, `series_analysis_plan`, `locked_config`, `scout`). The orchestrator forwards `max_series_refits` and `series_workers` by signature; `examine_data` already tagged cube directories `hyperspectral_series`.

### Files

- `scilink/agents/exp_agents/controllers/hyperspectral_series.py` (new): flattening, rows, outlier detection, refit selection, series file, trend controller (image trend codegen re-worded for cubes), synthesis prompt, report, scout, regime planning, schema completion, replay pool.
- `scilink/agents/exp_agents/hyperspectral_analysis_agent.py`: `analyze()` series dispatch and new parameters; `_analyze_series` driver; child-agent factory; `_synthesis_controllers`; `_compile_series_results`; guards for replay children.
- `scilink/agents/exp_agents/controllers/hyperspectral_controllers.py`: locked-targets branch in the plan short-circuit and decomposition bypass; `_replay_map_gate` replacing LLM reviews on replays.
- `scilink/agents/exp_agents/analysis_orchestrator_tools.py`: `series_workers` parameter, forwarding, schema.
- `tests/test_hs_series.py` (new, 32 tests): end-to-end driver with a stubbed single-cube pipeline and a real trend-script execution; regimes; salvaged/unverified rows; schema completion; scout on synthetic cubes; plan validation and retry; gate rules; locked-targets plan; light synthesis; parallel replays on a thread pool.
- `CLAUDE.md`, `README.md`: design notes and usage.

### Verification

- Offline: `tests/test_hs_series.py` 32 passed; full suite (`--ignore-glob='*_live*'`) identical to main (same 23 failures / 29 errors of environment noise), 2789 passed.
- Live on `bedrock/us.anthropic.claude-opus-4-8`: synthetic temperature series (regimes, alignment, flagging), drift with refit budget, interleaved states (incoherent axis, non-contiguous regimes), messy inputs (corrupt 2-D file, NaN block, 12-px cube, string filename-keyed values), two-cube series, orchestrator `run_analysis` on a sidecar-only directory (Tier-2 metadata extraction), and a dose series derived from `examples/eels_plasmons_demo/datacube.npy`. Replay timings: ~70 s per replay with the deterministic gate (≈5 min before), 3 replays in 92 s and 4 in 111 s on 3 workers, and zero collection wait with eager submission.

### Not changed

- The single-cube path and its output layout are untouched; a one-element list is a single analysis.
- Anchor and refit codegen (the LLM-heavy part) is unchanged; anchor variance remains the dominant cost.
- Human gating of the regime plan (co-pilot/autopilot) is not added yet; the planner runs autonomously.

## Second batch: replay gate, parallel replays, regime-plan human gate, data-facts codegen

**For the scientist**

- Replays are now about a minute per cube instead of five: a locked replay is judged on evidence (coverage, a non-collapsed map, values inside the anchor's plausible range) instead of a second round of LLM review, and replays run concurrently (`series_workers=3`) while the next regime's anchor is still being analysed.
- In co-pilot / autopilot the **regime plan is shown to you before anything runs**: the regimes, which cube anchors each, the control values, and the change-detection summary. Enter accepts; anything you type is sent back to the planner, which revises (live: "name the regimes 'baseline' and 'transition'" was honoured exactly, with the 500 K cube kept in its own regime).
- The first cube's code generation now starts from **measured facts about your data** (peak positions, prominence in sigma, width bounds, noise of the mean, flux per band) rather than textbook positions, so fit windows and measurability tests are set from the data.

**Technical**

- `_replay_map_gate` (`hyperspectral_controllers.py`): deterministic acceptance on locked replays; `replay_reference` carries the anchor's per-map stats; a rejection yields a locked-targets refit. Replay children skip the synthesis critic/editor (`_light_synthesis`).
- `ReplayPool` / `replay_worker` (`hyperspectral_series.py`): spawned-process pool (`SCILINK_HS_SERIES_POOL=thread` for tests), eager submission per locked regime, per-replay `dataset_NNNN/replay.log`, the parent's sandbox approval carried in the spec. `series_workers` on `analyze()` and `run_analysis`.
- `_regime_plan_gate` (agent) + `render_regime_plan` / `plan_series_regimes(feedback=…)` (series module): up to three revision rounds; skipped when human feedback is off or on a locked replay of a prior run; EOF accepts.
- `_render_data_facts` (`hyperspectral_controllers.py`): field-mean peaks via `find_peaks` with a local prominence window, over-wide "peaks" dropped, width by outward walk with saddle stops, smoothing adaptive to the noise of the mean; injected into `build_code_generation_prompt` ahead of the measurability gate together with the band-flux table. Hyperspectral QC goldens regenerated; verified the diff is exactly the inserted block.
- Outlier scoring refinements: primary outputs' mean/scalar columns only; per regime when regimes interleave; OLS prediction standard error with leverage and small-sample inflation; salvaged-but-unverified rows flagged and refit-eligible; candidates de-duplicated.

**Live (Bedrock Opus 4.8)**: serial 6/6 (replays 55–83 s), parallel 8/8 (3 replays in 92 s; with eager submission the replays finished before the regime anchor did), interleaved with 3 workers 11/11, co-pilot gate 10/10 (plan revised on feedback and accepted; five cubes in 17 min). Anchor codegen variance remains: with the data-facts block, two of three anchors locked on their second attempt, one exhausted its ladder on fit systematics unrelated to windows.

**Tests**: 36 in `tests/test_hs_series.py`; full suite identical to main.

## Third batch: mission control, regime-plan gate, anchor gates

**For the scientist**

- Through mission control (the meta agent), a series of cubes is one job: point it at the folder, and it delegates once; the reply is a per-condition table with the flagged change, and the delegation record carries the series claims and the feature table for planning.
- In co-pilot / autopilot the regime plan is shown for approval or revision before any cube is analysed.
- The first cube's code generation starts from measured facts about the data; a wrong "not measurable" call is corrected on the spot, and a script that silently returned empty maps is told exactly that.

**Technical**

- Meta routing: system prompt + `delegate_to_analysis` / `harmonize` descriptions steer a same-technique series to one delegation on the directory; `datacube_series_branches` + `FANOUT_SERIES_BUDGET_FACTOR` scale a fan-out branch that is a cube-series directory. `run_analysis` `series_workers` forwarding fixed (`UnboundLocalError` found live via the meta) with an orchestrator-level regression test.
- `_regime_plan_gate` (co-pilot/autopilot; Enter accepts, text revises via `plan_series_regimes(feedback=…)`, three rounds max, EOF accepts; plan rendered as the feedback `context`).
- `_data_facts` / `_render_data_facts` injected into `build_code_generation_prompt` with the band-flux table; `_NotMeasurableContradiction` repaired in the mechanical-correction loop when the facts say measurable; all-NaN required maps diagnosed in the retry critique. Hyperspectral goldens regenerated (diff verified to be the inserted block only).
- Tests: 41 in `tests/test_hs_series.py`; full suite identical to main (2798 passed).

**Live (Bedrock Opus 4.8)**: co-pilot gate 10/10 (plan revised on feedback, accepted; five cubes in 17 min); meta AUTONOMOUS over the sidecar directory → one delegation, one series run (5 cubes, 2 regimes, 3 workers), 500 K flagged, ledger with claims + `features.csv`, 25 min end to end; parallel_v3 8/8 with a failed anchor recovered by a locked-targets refit.

## Regression check of pre-existing functionality

Exposure is limited to seven hyperspectral and meta source files (no image, curve, server or UI changes); the controller diff is additive apart from a log label and an extended bypass condition. Full offline suite: identical failure set to main on every run (2799 passed on the final code); hyperspectral QC goldens regenerated twice with the diff verified to be exactly the inserted data-facts block and the new `coverage` field.

Pre-existing live suites rerun on Bedrock Opus 4.8:

| suite | result |
|---|---|
| single-cube plasmon demo (real 62×75×1496 cube) | success in 22 min, LSPR maps, 2 claims, approved record |
| `test_518_mask_replay_live.py` parts 1–4 (mask-scoped donor, follower replay, legacy degraded replay, harmonized meta fan-out + fusion) | 34/34 on the final code |
| `test_meta_fanout_hyperspectral_live.py` (image + cube complementarity gate) | pass |
| `test_hs_edit_adapt_live.py` | not runnable here (needs `~/Code/benchmarking_for_paper2` data absent on this machine); its supplied-script branch is the one #518's replays exercise |

The #518 suite caught two genuine regressions in the deterministic replay gate, both fixed and re-verified live: a fixed 50 % coverage floor rejected a mask-scoped follower that converges on 43 % of its dilated mask, and the constant-map rule rejected an emitter whose fitted centre is exactly constant. Coverage and constancy are now judged against the anchor's own map (every committed map records its converged fraction; the series passes it as the reference) and nothing is imposed without a reference.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `73a80a3b81fa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.