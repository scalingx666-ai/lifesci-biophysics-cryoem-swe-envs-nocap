# Issue

**Title:** Make the BO step index campaign-level and monotonic

## Description

.

## Summary

In a multi-iteration optimization campaign each iteration numbered its own step from 1, so iteration 2 was reported as step 1 and its diagnostic plots overwrote iteration 1's. The step index is now campaign-level: it continues from the last recorded step, including one set aside by a reset, so iteration 2 writes `step_2.png` and the earlier files stay. A reset no longer overwrites an earlier reset's backup and tells the agent which step comes next.

## Technical description

- `BOAgent._next_step_number(history)`: max recorded `step` in the live history + 1; with no live history, max step across `bo_history.json.backup*` + 1; else 1. `_stage_diagnostics` uses it instead of `len(history) + 1`, so all `step_N` artifacts (diagnostics plot, acquisition plot and data, batch CSV) and the history entry share the campaign index.
- A same-data re-run still pops the last entry and now keeps that entry's step number (`c.step_override`) so the replacement overwrites only the step it repeats.
- `reset_analysis_logic`: `optimization_data.csv` and `bo_history.json` are set aside as `.backup`, `.backup.2`, ... (never clobbering), and the response carries `next_bo_step`.
- Tests (`tests/test_bo_step_counter.py`, 5): continuation from live history and from backups (including an unreadable one), diagnostics naming after a backup, re-run override, and the reset tool's non-clobbering backups and reported next step. BO suites: 59 pass.

### Live check (Bedrock, real planning orchestrator)

Iteration 1 on nine points wrote `step_1.png`, `acq_step_1.png`, `acq_data_step_1.npz`. The `reset_analysis_logic` tool reported next step 2. Iteration 2 on ten points wrote the `step_2` trio; iteration 1's three files were byte-identical afterwards, the history held step 2 and the backup held step 1.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8c0f28fc5b6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.