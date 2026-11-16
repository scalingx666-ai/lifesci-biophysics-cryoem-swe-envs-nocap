# Issue

**Title:** Ground, audit and fully propagate the plan-mode TEA; accept multi-file data

## Description

## Summary

For a scientist using plan mode, the techno-economic analysis (TEA) now behaves like a real feasibility step instead of a one-line opinion:

- **Everything the TEA found reaches the plan.** Cost drivers, risks, the comparison to alternatives and, most importantly, the **data gaps** (what you would need to measure to make the economics quantitative) are handed to plan generation, refinement, portfolios and technical documents. Live, the first bench experiment the planner proposed targeted a listed TEA data gap.
- **You can see whether the numbers are real.** Every TEA now says whether it was grounded in your documents and literature (**strict**) or estimated from general industry benchmarks because your sources held no economic data (**fallback**). A fallback result is flagged in the chat reply, the HTML report, and every downstream prompt. An independent critic audits each quantitative claim against the evidence and lists its caveats. Live, it caught the author mixing a user price list with a literature price snapshot and comparing undiscounted oxide value against cost.
- **Give it all your tables at once.** `primary_data_set` accepts a folder or a comma-separated list, so a composition table, a price list and measured yields go into one assessment.
- A TEA run **after** a plan no longer resets the campaign's iteration counter or overwrites the plan card in the report.

## Technical description

### Design

- `OrchestratorTools._tea_context_block(heading=True)` is the single renderer of the stored `latest_tea_results` for downstream prompts (lists capped at 8, provenance line, critic caveats). `generate_initial_plan` injects it under its **own** `additional_context` key, never inside `user_context`, because that block is the user's hard constraints and the mandatory constraint-coverage note tells the author to map every item to a step. `refine_plan_with_results` appends it to `external_context` (not `literature_text`, so provenance stamping stays honest); `refine_portfolio` and `generate_ideation_portfolio` inherit via forwarding; `write_technical_document` passes it as `additional_context`.
- `PlanningAgent.perform_technoeconomic_analysis` calls `perform_science_rag(..., return_context=True, mode_key="generation_mode")`, stamps `res["grounding"]` (`mode`, `retrieved_context_chars`, `has_primary_data`, `primary_data_files`, `has_external_literature`), runs `planning_rag.critique_tea` (advisory, fail-open, dimensions `grounding|quantification|consistency|scope`, told which tier authored the result), stores sorted `critic_findings`, and writes `<stem>.grounding.md` next to the JSON. New `additional_context` parameter threads the previously ignored tool argument.
- `planning_rag.summarize_primary_data` accepts a dict or a list of dicts; one file keeps the byte-identical historical summary, several are joined under `## Data file i of N: <name>` headings (the per-file summary already opens with `### Experiment Data`). `perform_science_rag` skips the constraint-coverage note only for `TEA_INSTRUCTIONS`; every other caller is unchanged (existing test pins that).
- `OrchestratorTools._resolve_tabular_inputs` resolves comma lists and folders through `_resolve_data_path` (typo / session / extension recovery), de-duplicates, natural-sorts, refuses more than `_TABULAR_MAX_FILES = 10` with the file list. A single file still reaches the planner as a dict.
- `run_economic_analysis` stores `generation_mode`, `critic_findings`, `source_documents`, `primary_data_files` on `latest_tea_results` and returns `generation_mode`, `data_gaps_for_quantitative_analysis`, `caveats`, `grounding_record`, plus a `warning` on fallback. `run_task` `key_findings` carry the tier, the summary and up to five gaps (a record without a tier reads "provenance not recorded").
- `html_generator._render_tea` adds the comparison, data gaps, a provenance banner (strict / fallback / not recorded) and critic caveats; fields are coerced (`None`, bare strings, scalars) so a malformed assessment cannot crash the report. `generate()` keys cards on `(iteration, kind)`.
- Mid-campaign TEA: `perform_technoeconomic_analysis` only zeroes `iteration_index` when no plan exists; otherwise the TEA is stamped at the current iteration with stage `TEA Update`.
- Hardening: `format_caveats` tolerates non-list / non-dict findings; the block and run_task summary tolerate a non-dict `latest_tea_results` / `full_analysis`.
- Orchestrator prompt: POST-TEA PAUSE and TEA-FIRST rules now ask the agent to report `generation_mode` and pass every relevant table. CLAUDE.md gains a paragraph recording the contract.

### Files

`planning_rag.py` (summariser, critic, note scoping), `planning_agent.py` (TEA method), `orchestrator_tools.py` (block, tabular resolver, tool, consumers), `planning_orchestrator.py` (prompt, run_task findings), `html_generator.py`, `user_interface.py` (`format_caveats` guard), `instruct.py` (typo), `CLAUDE.md`, `tests/test_tea_grounding.py` (38 offline tests).

### Verification

- Offline: 38 new tests (multi-file summaries, tier recording, sidecar, critic on strict/fallback and fail-open, malformed critic output, context threading into plan/refine/document, tabular resolver edge cases and cap, header-only/binary files, author-error short-circuit, TEA-first vs mid-campaign numbering, full report with colliding iterations, damaged state, HTML escaping, run_task summary). Full offline suite diffed against an order-matched run on `main`: **zero branch-only failures** (the 59 shared failures are environmental).
- Live on `bedrock/us.anthropic.claude-opus-4-8` with FutureHouse literature search, credentials via environment only:
  - A strict tier, three tables, economic literature → plan → memo → refine: 31/32 (the miss was a test assumption; restore verified separately).
  - B fallback tier flagged end to end: 7/7.
  - C chat-driven autonomous TEA → plan; the model passed all three tables as a comma list unprompted: 5/6 (regex miss; reply named the tier and described the gaps).
  - D heading split verified in the actual author prompt: 9/9.
  - E1 portfolio with TEA 5/5, E2 run_task + restore 7/7, E3 TEA after plan + refine 9/9, E4 fallback TEA → plan 4/4, E5 image in knowledge folder 4/4.

### Not changed

`generate_initial_plan`'s own single-file `primary_data_set` handling; `refine_plan`'s literature stamping; the experiment-plan critic; every non-TEA `perform_science_rag` caller; MCP / CLI / web UI (no reader of the assessment shape exists outside `planning_agents`). Each TEA now costs one extra LLM call (the critic).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `913ba38a9176`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.