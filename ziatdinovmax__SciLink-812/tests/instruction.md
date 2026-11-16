# Issue

**Title:** Hyperspectral: keep mask and count maps, and hold every target to the outputs it was asked for

## Description

## What changes for you

Two problems could make a hyperspectral run end without the maps you asked for, even though it reported success.

- **A yes/no or count map no longer sinks the attempt.** If the analysis code returned a detection mask or a pixel count next to the real maps, drawing that one map crashed and the whole attempt was thrown away, good maps included. Such maps are now kept as ordinary numeric maps. If a map still cannot be drawn, only that map fails its review.
- **A run is held to the maps your request implies.** Sometimes the plan for a measurement did not say which maps it had to deliver. The quality checks then had nothing to enforce, and an attempt that returned only helper maps (a mask, a noise map) counted as success. Now, when a plan leaves this out, the agent writes it down in one extra step before the analysis code is generated. It uses the same rules the planner already follows. A purely exploratory request can still require nothing.

Plans that already list their maps are unaffected and make no extra call. In practice this covers Claude Opus 5.5, which always lists them.

**Live checks** ran the meta agent through the web UI's backend in autopilot, with every plan approved:

| | before | after |
|---|---|---|
| GPT-6 Astra: required maps written down | 0 of 3 runs | 4 of 4 runs |
| GPT-6 Astra: attempts lost to a yes/no map | 1 | 0 |
| Claude Opus 5.5: runs passing review (matched rerun) | 2 of 2 | 2 of 2 |

Stacked on #808. Merge that first.

---

## Technical details (for reviewers / framework maintainers)

**#809, non-float maps** (`RunDynamicAnalysisController._run_attempt`):
- **`_as_float_maps`** runs right after the script returns. Bool, int and uint maps become float. An object array is converted when it can be, and otherwise passed through to the existing checks. A list becomes an array. A float array comes back as the same object, so the float path is unchanged.
- **`create_feature_dashboard` is wrapped in `try`/`except`.** A failure adds a per-map `qc_failures` entry ("could not be plotted for review … return it as a numeric (H, W) array") and continues with the next map. It does not fail the attempt.

**#810, missing `required_outputs`** (`SelectRefinementTargetController._declare_required_outputs`):
- **When it runs:** only on the chosen custom-code target, when the target has no `required_outputs` and `state["analysis_objective"]` is set.
- **Prompt:** the objective, the target description, and the planner's own "Required outputs" rules section. That section is cut out of `SPECTROSCOPY_REFINEMENT_INSTRUCTIONS` by its markers, so the two cannot drift. A test fails if the markers disappear.
- **Reply:** JSON `{"required_outputs": [...], "reason": ...}`. Keys must be identifiers and duplicates collapse. An empty list stands, for an exploratory target.
- **Fails open:** a parse error or an exception logs `Required-outputs declaration skipped` and keeps the plan unchanged.
- **Downstream:** the declared keys feed the existing required-outputs gate and the existing not-measurable rules. No new gate is added.

**Tests:** `tests/test_hs_map_dtype_and_required_outputs.py` has 20 tests. They run the real controllers with the scripted-model harness, and the #809 tests draw real dashboards.
- **#809:**
  - float maps keep their identity;
  - conversions, and non-numeric values passed through;
  - a boolean mask next to a required map commits both, with one codegen call;
  - a flaky dashboard fails only its own map;
  - a float-only attempt is unchanged.
- **#810:**
  - the declaration is applied, and the rules appear verbatim in the prompt;
  - a declared target makes no extra call, and neither does a run with no objective;
  - an exploratory answer stays empty;
  - invalid and duplicate keys are cleaned;
  - a bad reply keeps the plan.
- **Each fix is checked against its tests:** disabling a fix makes its own tests fail.
- **Regression:** across the hyperspectral, series, verdict-parity and QC-golden suites, 342 tests pass. The 7 that fail also fail on `main`, unchanged by this PR: `test_hs_dynamic_records::test_retry_ladder_reaches_hot_and_succeeds` and the curve and image QC goldens.

**Live runs** (13 sessions):
- **GPT-6 Astra:**
  - every declaration came from the new step;
  - every boolean map was committed;
  - no dashboard needed the fallback.
- **Opus 5.5:**
  - the new step never ran;
  - in a first batch of 9 concurrent sessions, two Opus runs ended partial, both on reviewer rejections of the fits;
  - a matched baseline-vs-branch rerun passed on both sides.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72851d35e38c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.