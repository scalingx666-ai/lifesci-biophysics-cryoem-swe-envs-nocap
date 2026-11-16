# Issue

**Title:** Add derive_from_outputs: a sandboxed compute step over a completed run's outputs

## Description

.

## Summary

Until now the only code-capable path in analyze mode was `run_analysis`, which re-selects skills, re-plans and can drift into a different analysis, so even a simple "give me a table from what that run already computed" was forced through a full re-analysis. The analysis orchestrator now has `derive_from_outputs`: given a completed run (by id, or explicit paths) and a short description of the product, a small script runs in the sandbox over the run's saved files and writes the derived product to its own `results/derive_<id>/` directory. No skill selection, no planning, no best-of-N. It reformats, subsets, aggregates, converts units, joins, re-renders figures and extracts scalars; the orchestrator prompt routes "produce X from what run Y already computed" to it and keeps `run_analysis` for analyzing data.

I chose a distinct tool over a mode on `run_analysis`: the routing decision is then explicit by tool name, the two paths share none of their machinery, and each derivation gets its own record, receipt and script.

## Technical description

- `scilink/agents/exp_agents/derive_outputs.py` (new): `build_artifact_inventory` walks the sources (scratch/candidate folders skipped, capped at 120 files) and describes each artifact — array shape/dtype, table columns and first row, a nested structural outline of JSON (`json_outline`, lists transparent) so nested quantities such as `results[i].parameters.peak_1.amplitude` are locatable. `run_derivation`: (supplied or generated) script → `static_guard` (same denylist as data preparation, must use the runtime `_DERIVE` dict) → sandbox run with cwd in a scratch dir → `DERIVE_RESULT_JSON` marker → deterministic gate (`check_products`: every product exists, non-empty, under `out_dir`, written by this run) → sources are read-only (`source_violations`: a file the script created under a source is reported and removed, a modified one reported) → a model check of product previews against the task (`VERIFY_PROMPT`, two-vote gate as in `prepare_data`) → retry with the failure as feedback, up to `max_attempts`. Writes `scripts/derive_script.py`, `derivation_receipt.json` and an `analysis_results.json` (`agent_type: derivation`).
- `analysis_orchestrator_tools.py`: the `derive_from_outputs` tool (`task`, `analysis_id` default latest completed run, `source_paths`, optional `code`, `max_attempts`, `timeout_s`, `llm_verify`), sandbox approval, output under `results/derive_<id>/`, a record appended to `analysis_results` with `derived_from`, so `files_produced` and the delegation ledger see the products.
- `analysis_orchestrator.py`: tool item 6b and a note on step 7 (reshaping without new analysis goes to the new tool).
- Tests: `tests/test_derive_from_outputs.py` (12): inventory and outline, guard/marker/gate, supplied script without model calls, generate-retry-succeed with feedback, verification rejecting a hollow product then accepting the fix and a lone dissenting vote not costing a regeneration, giving up after max attempts, source protection (created file removed, modified file reported), the tool's record and schema, default-to-latest run and supplied code, plain errors, prompt routing. Full suite: identical failure set to main.

### Live checks (Bedrock, Opus)

- **Engine, real completed series run**: per-spectrum amplitude table, per-spectrum residual arrays plus overlay figure, and a crossover-temperature scalar — all three correct against the run's own fit results (420 K crossover), one attempt each.
- **Orchestrator**: a table request and an overlay-figure request both routed to `derive_from_outputs`, no `run_analysis`; values matched the fit results.
- **Meta path** (the issue's real path): the meta delegated the table request; the child derived it; the ledger listed the CSV.
- **Edge cases**: a quantity absent from the artifacts (honest per-row "not present", no fabricated numbers); an underspecified ask; a 302-file source with a corrupt array (inventory truncated, corrupt file described, correct product); a supplied script that timed out, one with a forbidden import, and one writing outside the output directory — each rejected and repaired on the next attempt, and the stray file removed from the source; two runs joined; an empty source (plain error).
- **Stress batch 2**: unit conversion (eV values verified), a join with an external user table, a 2×2 multi-panel re-render, an inverse-variance weighted mean with propagated error (verified), a filtered subset (3 of 4 rows), a table from Bayesian-optimization artifacts (a non-analysis output directory), two concurrent derivations on the same source, and a 3000×3000 column-mean reduction (verified). The source run was byte-identical afterwards.
- **Routing in both directions**: a request needing a genuine third-peak re-fit went to `run_analysis`; a request naming a non-existent run id recovered via `list_results` without re-analysis.
- **The issue's trigger, end to end** (ferroelectric STEM demo, #598 + #599): the verbatim request ran exactly one analysis and never chased a per-cell table. An explicit per-cell table request then went through `derive_from_outputs` only (four calls, zero re-analyses); the agent found the run's saved vector arrays mislabeled, refused to fabricate components, delivered the honest subset (positions and domain labels) and offered a re-run rather than launching one. That mislabeling is filed as a separate issue.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d093509fec2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.