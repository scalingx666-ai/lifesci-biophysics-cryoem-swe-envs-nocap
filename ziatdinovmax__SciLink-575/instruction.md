# Issue

**Title:** BO: say how a recommendation was selected; tighten physical_constraints

## Description

.

## Summary

Every BO result now says whether its recommendation is the acquisition optimum or a deliberate, constraint-aware deviation chosen by the LLM planner, and a planner-chosen point carries the optimum it deviated from. The planning orchestrator passes this through with a sentence the narration must state, so a point away from the plotted acquisition peak reads as intended rather than as an optimizer bug. The `physical_constraints` parameter is now documented as structural feasibility only; modeling caveats belong in `strategy_hint` or the objective.

## Technical description

- `bo_agent.py`: `_stage_constrained_batch` stamps `c.selection_method` (`constrained_planner` on planner success, `acqf_optimizer` otherwise incl. the fallback); `_stage_output` emits `selection_method`, and for a planner choice `acqf_optimum` (the unconstrained recommendation / batch) plus, for a single point, `deviation_from_acqf_optimum` per parameter; `_stage_record` writes the method into `bo_history.json`. Docstring tightened.
- `orchestrator_tools.py`: the tool description tightened the same way; the response carries `selection_method`, `acqf_optimum` (categorical levels decoded), `deviation_from_acqf_optimum` and a `selection_note`.
- The planner is deliberately NOT skipped for `batch_size == 1`: a discrete-levels constraint applies to a single point as much as to a batch, so dropping it there would silently return an infeasible optimum. The invariant the issue asks for — the point is the optimum OR is clearly marked as a deviation — holds through the stamp.
- Tests (`tests/test_bo_selection_method.py`, 4, no LLM): unconstrained → optimizer; planner choice → marked with the optimum and per-parameter deviation; planner failure → optimizer fallback; batch carries the whole unconstrained batch. Existing BO suites: 37 pass.

### Live check (Bedrock, Opus 4.8, plan session)

8-row catalyst/temperature dataset, one BO step with `physical_constraints="only 0.1, 0.5, 1.0, 2.0, 5.0 mM available"`, batch 1. The tool response carried `selection_method: constrained_planner`, `acqf_optimum`, `deviation_from_acqf_optimum` and the `selection_note`; `bo_history.json` records the method; the narration stated plainly that the point was chosen by the constraint-aware planner and not the acquisition maximizer. The same run surfaced a pre-existing bug the new fields made visible — the categorical-encoded catalyst column let the planner mix code-space bounds with mM data and its physical answer was decoded as a level index — filed as #579.

### Live matrix (Bedrock, Opus 4.8, plan sessions, continuous 12-row dataset)

- unconstrained, batch 1 → `selection_method: acqf_optimizer`, no note;
- discrete bath temperatures (45/55/65/75 °C), batch 1 → `constrained_planner`, `acqf_optimum` (82.7 °C) and `deviation_from_acqf_optimum` (−7.7 °C) present, note carried, narration stated the planner selection;
- shared-temperature batch of 4 → `constrained_planner` with the whole unconstrained batch as `acqf_optimum`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.