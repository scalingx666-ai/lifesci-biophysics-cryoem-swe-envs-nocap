# Issue

**Title:** A hyperspectral plan with nothing to run is asked for again, then reported

## Description

## What changes for users

When a spectrum-image analysis goes straight to per-pixel measurement (no decomposition), SciLink first writes a plan of what to compute. In a live run the plan came back with nothing to compute, even though its own reasoning said a measurement was needed. Nothing was measured, a report was still written, and the run was marked a clean success.

Now:
- **SciLink asks for the plan once more** when it has nothing to compute, showing the model its own plan. If the second answer is no better, the first one stands.
- **A run that still measures nothing says so.** Its status is `partial` with confidence `none`, with a warning that no maps or measurements were produced (or that the planning step itself failed). The meta agent and any other caller see that instead of a success.

Nothing changes for runs whose plan already has a measurement, for runs that decompose first, for replays of a saved analysis, or for a plan a person revises at the review step.

Checked live: 3 runs of a low-loss spectrum-image example through the meta agent on this branch all produced their maps (11 to 23 min, $3.25 to $5.27). The original failure happened in 1 of 3 runs, so the retry did not fire there; the unit tests cover the failure itself.

---

## Technical details

- **`SelectRefinementTargetController`** (`hyperspectral_controllers.py`): in skip-decomposition mode, a parsed plan with no `custom_code` target triggers `_ask_for_custom_code_target`: one more call with the same prompt parts, the previous answer, and one sentence stating the principle (a `custom_code` target is the only way anything in this plan is measured). The second answer is used only if it parses and has a `custom_code` target; otherwise, or on an exception, the first answer stands. `_has_custom_code_target` is the shared check.
- **Planner failure branches** now set `planning_failed: True` on the decision.
- **`RunDynamicAnalysisController`**: when the gate finds no `custom_code` target and `skip_decomposition` is set, it appends a degradation note (`kind: withheld`, `confidence: none`, `missing_required: []`) whose caveat names an empty plan or a failed planner. The existing response assembly turns it into `status: partial`, `confidence: none` and a warning. Decomposition runs are untouched: their maps are the result, and no refinement is not a failure.
- **Unchanged paths**: locked-script replays (`reuse_records`, including strict replay, which still makes zero model calls) and `locked_targets` return before the planner call. The human-feedback replan in `IterativeFeedbackController` (shared by all agents, a person in the loop) is unchanged.
- **Known limitation, follow-up**: the holistic synthesis runs before the note is attached to the result, so the written report can still describe the data when nothing was measured. Only the status, confidence and warnings change here.
- **Tests**: `tests/test_hs_empty_plan.py`, 10 tests, driving the real controllers: retry used when it helps, first answer kept when the retry is empty or raises, no retry when the first plan has a target or decomposition ran, note recorded only in skip mode, planner-failure caveat, zero planner calls for replays and locked targets, and the note turning an `analyze()` result partial. The 4 tests of new behaviour fail on `main`. Hyperspectral suite: 245 passed; `test_hs_dynamic_records.py::test_retry_ladder_reaches_hot_and_succeeds` fails on `main` as well.
- Reviewed by an independent agent before opening; its two applicable findings (planner-failure caveat, missing tests) are included.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ca1343a708c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.