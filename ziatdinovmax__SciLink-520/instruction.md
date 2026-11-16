# Issue

**Title:** Reproduce a mask-scoped donor's fit scoping on harmonized replay

## Description

.

When one dataset's approved analysis is replayed on its sibling datasets (harmonized mode), the replay used to keep the donor's exact arithmetic but silently lose its *scoping*: a donor that had restricted its per-pixel fit to a small region of interest was replayed over the entire frame. The region's signal drowned in background, the follower's quality checks rightly rejected the result, and nothing warned that the pipelines were no longer actually the same.

This change makes the scoping travel with the script. The donor's region can't be copied pixel-for-pixel — in a sibling dataset the feature of interest generally sits somewhere else — so what is stored is the *rule* that defined the region: the spectral signature of the component that carries the signal, plus the same threshold-and-dilate procedure. On replay, the follower applies that rule to its **own** data and recovers the equivalent region at its own location. If the rule can't be applied (an older record made before this change, or incompatible data), the replay proceeds full-frame but is flagged loudly at every level — in the log, on the per-target record, on the analysis result, and in what the fan-out branch reports to fusion — so a degraded harmonization is a visible caveat, never a silent methodological drift.

Validation on live Bedrock runs with synthetic sibling datasets (the inclusion deliberately placed at different locations, its intensity ramping across the series):

- **Pre-fix baseline** (run accidentally against the old code, kept as evidence): the follower replayed verbatim, fit the full frame, and failed QC with no warning — the exact symptom in the issue.
- **Post-fix**: the follower's re-derived region centers on its own inclusion at matching coverage (2.9%), and the replay passes QC verbatim — 25/25 checks.
- **Legacy-donor path**: with the recipe stripped from the donor record, all four degraded-harmonization surfaces fire.
- **End-to-end fan-out** (`harmonize=True`, three-dataset series): both followers replay with scoping reproduced, and fusion correctly recovers the planted intensity ramp with a stable line center.

---

**Technical details**

Three linked spots in `hyperspectral_controllers.py`, per the issue:

1. `_build_target_record` — approved `component_mask` targets persist `fit_mask_recipe` `{fit_scope, mask_component_index, component_spectrum, mask_fraction}`; the endmember comes from `state["final_components"]` (same 1-based "Component N" contract as `_build_fit_mask`).
2. `SelectRefinementTargetController` (reuse branch) — replay targets carry `fit_mask_recipe` plus `fit_scope`/`mask_component_index` alongside `supplied_script`.
3. `RunDynamicAnalysisController.execute` — on a `supplied_script` target with a recipe, `_rebuild_fit_mask_from_recipe` projects the follower cube onto the donor endmember (clipped least-squares coefficient) and feeds the abundance-like map through the identical `_build_fit_mask` half-max + dilation path; the recipe is re-persisted (coverage restated) so chained replays keep the scoping. Storing the endmember rather than a component index avoids the cross-cube component-ordering instability of re-running a decomposition.

Degraded detection: a recipe-less donor whose script's `analyze_feature` signature declares `fit_mask` (regex on the frozen script) or an unusable recipe sets `replay_scope_degraded` on the record, logs `DEGRADED HARMONIZATION`, adds `scope_degraded`/`scope_warnings` to the response's `script_reuse` block and `warnings` (appended after the partial-status block, which assigns `warnings`), and surfaces `script_reuse` + a `reuse_warning` in the `run_analysis` tool response.

No-regression evidence: full offline suite A/B vs clean `main` has identical failure sets (passed-count delta is exactly the 8 new tests); HS prompt goldens 4/4 (prompts byte-identical on existing paths); all new record/response fields are additive and emitted only when a mask is involved. One pre-existing failure (`test_hs_dynamic_records.py::test_retry_ladder_reaches_hot_and_succeeds`) reproduces identically on clean `main`.

Tests: 8 new offline tests in `tests/test_hs_locked_replay.py` (15/15 total); live driver `tests/test_518_mask_replay_live.py` (donor / follower / legacy-degraded / fan-out harmonize, 25/25 + 9/9). In the fan-out run one follower's per-map QC salvaged its result over a physics-claims critique — the judge's independent verdict on that cube, orthogonal to this fix (its scoping was reproduced verbatim).

Not addressed here: #519 (harmonize fallback on a partially-successful donor — separate, cross-layer).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0f722b366d13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.