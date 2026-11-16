# Issue

**Title:** Merge split feature-table columns, name dropped units, and replay ingestion on continuations

## Description

## Summary

Two things change for someone running an analysis → Bayesian-optimization campaign.

**A unit is no longer silently lost between analysis and BO (#534).** When the per-spectrum fitting scripts report the same quantity under two names (say `metric` for one spectrum and `metric_value` for the rest), the feature table now folds them into one column. If a column is still empty for some units, the analysis result says which units, and the planning side names the units it has to skip instead of just counting them. The optimizer never trains on a silent subset of your data anymore; you are told exactly which measurement is missing and why.

**Adding one more measurement to an existing campaign is instant again (#535).** Appending a same-schema result file no longer sends the scalarizer back through code generation and its retry loop. The campaign already knows its input and target columns, so the new row is read directly. In the live runs below, every continuation completed with zero scalarizer LLM calls, including after a checkpoint restore.

## Technical description

### #534 — split objective across sibling columns

- `feature_table.py`
  - `merge_variant_columns(rows)`: groups column names by identity (lower-cased tokens with trailing `value`/`val` stripped), merges a group only when no unit populates more than one member (complementary sets), keeps the majority-populated name (first seen on tie) so a target a caller already chose stays valid, logs the moved units. Statistics suffixes (`mean`, `std`, `err`) and unit tokens are deliberately not variants. Called from `write_feature_table` before the column union.
  - `describe_feature_table` gains `warnings`: a column empty for some units (units named, elided after 6) and a pair of partially-populated columns whose populated sets are disjoint and jointly cover all rows.
- `analysis_orchestrator_tools.py`: `run_analysis` response carries `feature_warnings`. `analysis_orchestrator.run_task` folds them into the result's `warnings` (they already ride `feature_tables_schema` into the meta ledger). Meta system prompt: one sentence telling the model to name affected units instead of calling a table BO-ready.
- `scalarizer_agent._try_table_passthrough`: result carries `units` (the `unit` column) alongside the requested columns; an exact case-insensitive column match now wins over the token-subset match (with `metric` and `metric_value` both present, a request for `metric` read `metric_value` because it came first in the header — found in live stress test E).
- `orchestrator_tools.analyze_file`: unit labels are aligned to `df_new`'s index (from a `unit` column or `res["units"]`) and realigned by index after dedup slicing, so `rows_skipped_units` names the skipped rows; the `warning` text states the optimizer will not see them.

### #535 — continuation drops to codegen rediscovery

- `analyze_file`: when the caller omits `inputs`/`targets`, no script is being reused, and `expected_input_columns`/`expected_target_columns` are established, `_schema_requirements` and `column_role_hints` are armed from the campaign schema. Inputs whose keys appear in the file's sidecar JSON are excluded (they are merged after scalarization), via a new `_sidecar_scalar_keys` helper. The schema is already checkpointed, so this is sticky across restore without new state.
- Adjacent fix: the explicit-schema arming keyed on `not has_locked_script`, so `force_regenerate=True` with a locked script generated a new script without the caller's schema. It now keys on `script_to_use is None`.
- `_try_table_passthrough` goal-derived path: a derivation term that is a whole token of an existing column (`yield`, `selectivity`, `rate`) no longer vetoes; the veto stays for terms the table cannot satisfy.

### Tests

- `tests/test_analyze_file_continuation.py` (new, pytest, drives the real tool with a stubbed `scalarize`): first ingestion arms nothing; continuation arms from campaign schema; sidecar inputs not demanded; survives checkpoint restore; locked script still wins; force_regenerate sees explicit schema; skipped rows named from pass-through `units`; labels follow the dedup slice.
- `tests/test_feature_table_describe.py`: warnings shapes, majority-name merge, tie keeps first-seen, both-populated not merged, statistics suffixes untouched, merge notes.
- `tests/test_scalarizer_passthrough.py`: literal-column veto exemption, `units` on the result, exact-name priority.
- Existing suites re-run green: analyze_file shapes, ingest-missing-values, input types/bounds/directions plumbing, candidate pool, stale session dir, MCP restart durability, fusion numerics, hyperspectral QC.

### Live verification (Bedrock `us.anthropic.claude-opus-4-8`)

- Seeded campaign by explicit pass-through, then continuations by direct tool call and by the model's own chat-driven call: pass-through fired every time, zero scalarizer LLM calls.
- Stress scenarios: reordered/extra/case-variant columns; checkpoint restore into a fresh orchestrator; two-target campaign with swapped columns; a continuation whose target must be derived (pass-through declined, armed codegen produced the correct selectivity first attempt with the sidecar input excluded); user naming the minority sibling column (8 units skipped by name, the model reset and re-ingested on the populated column); meta with a split-column table (read the warning, delegated to planning, which coalesced the columns and fit on all 9 units, and named the affected spectrum).

### Not changed

- No new orchestrator state for ingestion stickiness; the checkpointed schema is the record.
- The planner-bounds exact match in `run_optimization` (#67) is separate and follows in its own PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78704328ded5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.