# Issue

**Title:** Release 0.0.73

## Description

## Summary

A fix release on top of 0.0.72.

- **Bayesian optimization with categorical inputs and physical constraints** (#583): the constrained planner now reasons in one space. A categorical input (stock concentrations, solvents) is shown to it as its list of allowed levels, together with the acquisition landscape, data summary, current best and unconstrained picks in physical units, and every value it returns is mapped back to the optimizer's encoding before the recommendation is reported. The recommendation, the BO history and the narration can no longer name different levels, and the planner no longer invents exclusions from index-space bounds.
- **Web server plotting on macOS** (#583): the server forces a headless matplotlib backend at startup, so a plan session's optimization step no longer fails at its diagnostics plot on a worker thread.
- **Web UI** (#584): a smaller Mission Control / Analyze / Plan picker on the landing page, and readable Arguments / Result blocks in the Telemetry tab and the memory panel's diff in light mode.

## Technical description

\`bo_agent.run_optimization_loop\` takes \`input_levels\` (the orchestrator's level maps); the constrained stage renders bounds, landscape, data summary and reference points in level names and maps the planner's answers to codes (exact level name, numeric-looking values compared as numbers, or the nearest numeric level within 1 % of the spacing; otherwise a validation error). \`scilink-web\` and \`create_app\` set \`MPLBACKEND=Agg\` and \`matplotlib.use("Agg")\` before any agent imports pyplot. CSS-only changes for the picker sizes and the code-pane text color. Tests: 5 new for the planner space, 1 for the backend; BO suites at 55.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff19d0777bf1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.