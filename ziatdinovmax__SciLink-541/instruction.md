# Issue

**Title:** Resolve plan parameter names onto input columns for BO bounds

## Description

## Summary

When a plan says a knob is called `Temperature` with a range of 20 to 120 °C, and your data file calls that column `temperature_C`, the optimizer now uses the plan's range. Before, it quietly fell back to the range spanned by your existing measurements, so the search never left the region you had already explored, and the assistant then asked you for limits the plan already contained. The same applies to categorical knobs: a plan-declared solvent list (including solvents you have not tried yet) reaches the optimizer under whatever the column is called.

When a plan parameter cannot be mapped safely, for example `Temperature` when the data has both an inlet and an outlet temperature, nothing is guessed. The tool reports the range that was not applied and the columns it could belong to, and the assistant tells you so and asks which one you meant.

.

## Technical description

### Change

- `orchestrator_tools._resolve_plan_params(plan_names, columns)` maps each `optimization_params` name to an input column through four rungs, each applied only to names and columns not yet claimed: exact name; case-insensitive name; equal token sets with unit words dropped (`_PLAN_UNIT_TOKENS`); token subset in either direction. A rung accepts a pair only when the name has exactly one candidate and that column is wanted by exactly one name. Returns `(resolved, unmatched, candidates)` where `candidates` lists the columns an unmatched name resembled, for the ambiguity message. Word tokens rather than `difflib` so that `Temperature` finds `temperature_C` and `Reaction time (min)` finds `time_min`, while `T` never token-matches `temperature_C`.
- `run_optimization`: after the plan loop, `scientific_bounds` and `planner_levels` are re-keyed onto the resolved columns. Precedence is unchanged: categorical, then caller `input_bounds`, then planner, then data. The PLANNER log line names the plan parameter it came from; the response adds `plan_param_matches` (column → plan name) when any name differed. Unresolved parameters go into `input_bounds_warnings`: with a range and a ready-to-pass `input_bounds` when ambiguous between named columns, with the range when nothing resembled it, and a note that the level universe is unused for an unmatched categorical.
- `planning_orchestrator` system prompt: one sentence asking the model to state plainly when a plan parameter's range was not applied, re-run with `input_bounds` when the mapping is clear, otherwise ask.

### Tests

`tests/test_plan_param_matching.py` (13 cases): resolver rungs and both-ways ambiguity (equal tokens disambiguate `temperature_C` from `temperature_ramp_C`; `time` vs `Time (min)` are the same name once the unit drops and neither is guessed; single-letter and unit-only names never token-match; case-sensitive exact wins before case-insensitive); integration through the real tool with a stubbed BO loop: planner bounds and a plan-declared unobserved level apply through name variants, unmatched parameter warned with its range, caller bounds still win, exact names unchanged with no `plan_param_matches` key, ambiguous name reports both candidates. Existing input-bounds, input-types, target-directions, candidate-pool, BO seed, prompt snapshot, and MCP suites re-run green.

### Live verification (Bedrock `us.anthropic.claude-opus-4-8`)

- Real `generate_initial_plan` produced parameters named `Temperature` and `Time`; both resolved to `temperature_C` and `time_min`. The model also passed the chat-stated ranges as caller bounds, which correctly took precedence.
- Injected plan with `Temperature`, `Reaction time (min)`, and an unmatched `Pressure (bar)`: both matched columns bounded from the plan, the model flagged the pressure parameter to the user and did not ask for a temperature range.
- Plan-declared categorical `Solvent` with an unobserved `MeCN` level on a column named `solvent`: three levels reached the optimizer.
- Ambiguous `Temperature` against inlet and outlet temperature columns: nothing applied, the model stated the caveat plainly with both column names and asked. Before the prompt sentence it only offered to constrain bounds "if you want".
- Checkpoint restore in a fresh orchestrator keeps the matching.

### Not changed

Caller `input_bounds` validation, the data-derived fallback and its warning, and the MOO narrowing path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78704328ded5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.