# Issue

**Title:** BO: the constrained planner works in one space for categorical inputs

## Description

.

## Summary

When a BO input is categorical (a set of stock concentrations, a choice of solvents) and physical constraints are given, the constrained planner used to see index-encoded bounds next to data in physical units, could invent exclusions like "the 5.0 mM stock exceeds the 4.0 upper bound", and its physical answer was then decoded as a level index, so the reported recommendation could name a different level than the one it chose. The planner now reasons entirely in physical terms — it sees each categorical input as its list of allowed levels, the acquisition landscape, data summary, current best and unconstrained picks in level names — and everything it returns is mapped back to the optimizer's codes before the orchestrator decodes it once. A second fix found on the way: the web server now forces a headless matplotlib backend at startup, so a plan session's BO step no longer fails at its diagnostics plot on macOS when no image preview happened to switch the backend first.

## Technical description

- `bo_agent.run_optimization_loop(..., input_levels=None)`: the orchestrator passes its `level_maps`. In `_stage_constrained_batch` the acquisition-landscape table renders categorical region centres as level names (`_summarize_acquisition_landscape(input_levels=...)`), the data summary comes from `_physical_frame` (codes → level names, numeric levels as numbers), and current best / Pareto front / unconstrained recommendations go through `_decode_point`. `_plan_constrained_batch(input_levels=...)` renders a categorical input's bounds line as `CATEGORICAL — one of [...]` and, on validation, maps each returned value to a code via `_code_of` (exact level name with numeric-looking values compared as numbers, or the nearest numeric level within 1 % of the level spacing; anything else is a validation error naming the levels). Recommendations, `next_x_batch`, `acqf_optimum` and `bo_history` stay in code space; the orchestrator's existing `_decode_categorical_recs` is now applied to codes only.
- `scilink/server/cli.py` / `app.py`: `_headless_matplotlib()` sets `MPLBACKEND=Agg` and `matplotlib.use("Agg")` before any agent imports pyplot (also from `create_app`, for embedders and tests).
- Tests: `tests/test_bo_categorical_planner_space.py` (5): code/level mapping incl. snapping tolerance and string levels; the planner prompt shows levels and its physical answer becomes a code; an off-level answer is rejected with the level list; level names in the landscape table and data frame; the stage hands the planner physical inputs and keeps codes downstream. `test_web_server.py`: the app leaves matplotlib on Agg. BO suites: 55 pass.

### Live matrix (Bedrock, Opus 4.8, plan sessions)

- Original repro (8 rows, five stock levels, constraint text, batch 1): planner chose 1.0 mM at 80 °C; `bo_history` holds code 2.0, the tool response decodes it to `"1"`, the narration says 1.0 mM — one answer everywhere, and the planner's trade-offs no longer mention a fabricated bound.
- Same data, unconstrained: `acqf_optimizer`, code 3.0 → `"2"` mM, consistent.
- Same data, batch of 4 sharing one stock: all four at code 3.0 → `"2"` mM with four temperatures.
- String levels (solvent water/ethanol/DMSO, ethanol banned, batch 2): planner returned DMSO and water; codes 0 and 2; ethanol excluded; decoded names correct.
- The first attempt of this matrix failed on the matplotlib GUI-backend error on all four sessions, which is what led to the server fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c18528096c14`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.