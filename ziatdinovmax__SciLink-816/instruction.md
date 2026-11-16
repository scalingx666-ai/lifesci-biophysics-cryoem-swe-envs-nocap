# Issue

**Title:** Hyperspectral: name regions by evidence, and show the planner the field

## Description

## What this fixes

When you ask the hyperspectral agent for a per-pixel measurement (a peak position or intensity map), it skips decomposition and goes straight to planning a custom analysis. Until now the planner did that without ever seeing the field: its prompt said "(No visual results available)". When the request involved regions (particles against gaps, a film against its substrate), it had to guess which was which. The guess was nearly always "the brighter side is the material". Where the material is actually the darker side, every region average came out under the wrong name. The run's own summary then contradicted the maps it had just made, and the meta agent could only report the contradiction.

Two changes:

- **The planner now sees the field.** With decomposition skipped, the planner gets one picture made directly from the data, with no model involved: the total signal across the field, plus the signal integrated over each main feature of the field-mean spectrum. It also gets the "DATA FACTS" block that the code writer already received (measured peak positions, widths and noise). It's the first look a scientist takes before naming regions. No extra model call is made.
- **Regions are named by evidence.** A region called "particle", "substrate", "vacuum" or a phase has to be identified from evidence in the data, and the script has to say what that evidence is. When the data can't tell which is which, regions are named by their signal ("high", "low") instead.

Runs that do run decomposition see their component maps as before; their planner prompt changes only by the one sentence. If you give feedback at the plan approval step, the revised plan is made with the same picture of the field.

**Measured live** (meta agent, autopilot, Opus 5.5), on a spectrum image of a particle array where the particles are darker than the gaps between them:

| | Region labels swapped |
|---|---|
| `main` | 3 of 3 runs |
| Principle only | 1 of 3 wrong, 1 partly right, 1 right |
| Principle + field overview (this PR) | 0 of 3. One run named the dark domains as the particles "by shape"; two kept signal names with the right physical reading. |
| This PR, with feedback typed at plan approval ("Also compare the plasmon between the particles and the gaps.") | 0 of 2. Both revised plans read the particles as dark and the gaps as bright. |

Controls on four other spectrum images (core-loss EELS, cathodoluminescence and Raman):
- answers matched `main`;
- times stayed within `main`'s range, except one Raman run in three that needed a second analysis after a masking failure. The other two Raman runs took 33 and 41 min (`main`: 39).

---

## Technical details (for reviewers / framework maintainers)

### Review follow-up (commit 3108e70d)
- **Replan after feedback.** `SelectRefinementTargetController` keeps `state["field_overview"] = {image, caption, text}`. `IterativeFeedbackController` (`base_controllers.py`) appends it when the run has no `analysis_images`. A dedicated key is used rather than `analysis_images`, which the report and synthesis also read. Other agents never set the key, so the shared controller is unchanged for them.
- **The planner gets facts, not codegen instructions.** `_data_facts` also returns `facts_text`: the facts without the closing paragraph addressed to the code writer (windows, seeds, not_measurable). The planner and the replan get `facts_text`; the code-generation prompt is byte-identical.
- **A dip panel shows depth.** A dip feature is drawn as its depth below a straight baseline between the window's edges, pixel by pixel, so a stronger band is brighter. Peak panels are unchanged.
- **Tests:**
  - `test_a_replan_after_feedback_sees_the_same_field` runs through the real pipeline: planner, then typed feedback, then replan, which must carry the same overview image.
  - `test_the_planner_gets_the_facts_without_the_code_writers_instructions`.
  - `test_a_dip_panel_is_brightest_where_the_band_is_deepest`.
  - All three fail on 367c7943. Hyperspectral, principle and feedback-question tests: 414 of 415 pass; `test_retry_ladder_reaches_hot_and_succeeds` also fails on `main`.

### Changes
- `scilink/agents/exp_agents/_region_names.py`: `REGION_NAMES_PRINCIPLE`, one sentence with no braces, in the same style as `_input_integrity`.
- `instruct.py`: `SPECTROSCOPY_REFINEMENT_INSTRUCTIONS = _with_principle(..., REGION_NAMES_PRINCIPLE)`, attached after every template is defined, like the `_input_integrity` principles. It reaches `SelectRefinementTargetController.instructions`, the pipeline factory's `IterativeFeedbackController(refinement_instruction=...)` and `_required_outputs_rules()`, which quotes a section further down and is unaffected.
- `hyperspectral_controllers.py`:
  - **`RunDynamicAnalysisController._render_base_prompt`:** `with_principle(base_prompt, REGION_NAMES_PRINCIPLE)` after `CODEGEN_PRINCIPLE`. Every attempt, annealing level and retry is rebuilt from this builder (`ctx.base_prompt_builder`).
  - **New `_field_overview(state, max_features=4)`:**
    - It resolves the axis with `resolve_axis_spec` / `signal_axis_values`, the same rule as dynamic analysis.
    - It calls `_data_facts(...)` and renders one JPEG: `nansum` over the axis, then each of up to 4 field-mean features integrated over `position ± max(width/2, 1.5 steps)`, with 1–99th-percentile scaling.
    - It returns `{"image", "text", "n_panels"}`, or `{}` on any failure. It is advisory and never raises.
  - **`SelectRefinementTargetController.execute`:** in skip-decomposition mode with no analysis images, it appends the overview image, a one-line caption ("Brightness is signal level, not what a region is") and the DATA FACTS text, where it used to print "(No visual results available)". Outside skip mode, or if the overview fails, the old path runs unchanged.

### Tests
`tests/test_hs_region_names.py`, 9 tests (the review follow-up adds 3, listed above). The original 6:
- **The prompts actually sent carry the principle:**
  - the planner's first model call, through the real pipeline factory, with the principle within the first 400 characters;
  - both the first and the retry code-generation prompts, where the first attempt returns a required map that is all NaN.
- **The principle names no technique.**
- **A skip-mode planner sees the field:** exactly one JPEG plus DATA FACTS, and no "(No visual results available)".
- **A planner with decomposition maps is unchanged:** no overview, and the same images.
- **`_field_overview` never raises.**

The hyperspectral and principle test files pass (261 + 6), except `test_hs_dynamic_records.py::test_retry_ladder_reaches_hot_and_succeeds`, which also fails on `main`.

### Live evidence (raw)
- **Particle array, 3 runs per arm.**
  - The planner prompt on `main` says "(No visual results available)" in all 3 runs, and all 3 use an Otsu threshold with the bright side labelled "particle" / "interior".
  - With the principle alone, 1 of 3 runs is right. That run was the only one that ran decomposition first and so saw component maps. The two skip-mode runs were wrong or partly right.
  - With principle + overview, all 3 runs are skip-mode and none is swapped.
  - Time and cost: `main` 11–18 min / $3.3–4.6; this PR 16–24 min / $4.1–6.6. These runs fitted more per pixel: two peaks plus a background term.
- **Controls (this PR, skip-mode or not):**
  - core-loss EELS ×2 (skip-mode, so they saw the overview): same answers, 12.9 / 13.6 min (`main` 12–21);
  - cathodoluminescence ×1 (decomposition): same time and cost, weaker exciton-ratio maps;
  - Raman ×3 (decomposition): 33.4 / 40.9 / 78.2 min. In the 78-minute run, the first analysis failed its map review 4 times (off-flake noise pixels passed a per-pixel quality cut), and a second analysis passed.

### Known limits
- The overview is shown only in skip-decomposition mode. A run that decomposes relies on its component maps.
- An auxiliary structural image (HAADF, topography) remains the strongest evidence for region identity. It still reaches the planner as before; when it has the cube's exact (H, W) it also reaches the code as an operand. Spatial resampling of an auxiliary map onto the cube's grid is not part of this PR.
- Sample sizes are small (3 runs on the target case, 1–3 per control).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29970e0e0892`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.