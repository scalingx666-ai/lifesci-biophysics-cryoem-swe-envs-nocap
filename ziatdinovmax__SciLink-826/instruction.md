# Issue

**Title:** Headless results: say whether the task failed and whether its analyses were verified

## Description

## Summary

When SciLink is run from a script or by another agent (`scilink -p "task" --output-format json`), the caller decides what to do next from the exit status and the JSON. Two things made that unreliable:

- **Mission Control called a turn a success whenever it produced a reply.** A turn in which every delegated analysis failed still reported `success` and exited 0. A resumed session also reported files from its earlier turns as this task's.
- **`status: success` for an analysis means "it ran", not "the fit was approved".** A salvaged or unverified result looked the same as a verified one unless the caller walked every row of the result.

What changes for you:

- The JSON of an analysis task has a top-level **`verification`**: how many analyses ran, how many were verified, which were not and why, which failed. Text and markdown output print the same count.
- Mission Control's result is built from what the turn actually did: `status` is `success`, `partial` (some delegations failed, some did not) or `error` (the turn failed, or every delegation did). It lists each delegation with its status and its verification.
- A new **`--strict`** flag for tasks that are meant to analyse data: exit status 3 unless at least one analysis ran, all were verified and no delegation failed. This also catches the case seen in testing where the data file did not exist, the agent replied "file not found", and the run exited 0.
- Without `--strict`, exit statuses are what they were (0 or 1), except that a Mission Control turn which truly failed now exits 1.

## Technical description

### Design

- **`exp_agents/_verification_record.py`**: `verification_summary(rows)` counts a task's `analyses` rows into `{analyses, verified, unverified[{analysis_id, reason}], failed[{analysis_id, status}], all_verified}`. `all_verified` is `None` when no analysis ran. Rows of `DataPreparation` / `OutputDerivation` that succeeded are not analyses (they carry no verification record, so their `verified` is False by construction) and are not counted; a failed one is listed under `failed`. `merge_verification` combines several tasks' summaries: `all_verified` only when every part's is, parts with no analysis counted as `without_analysis`, items tagged with their `delegation` index.
- **`AnalysisOrchestratorAgent.run_task`** adds `result["verification"]`. Nothing else in the result changes; `status` keeps its meaning.
- **Meta ledger**: `_close_delegation` stores the child's `verification` on the entry (round-trips through the checkpoint; old checkpoints read as `None`). `chat()` records `_last_chat_error` (reset at the top of every turn, set in the catch-all), mirroring the analysis orchestrator.
- **`meta_agent/turn_result.py`** (new, pure): `turn_entries` = entries appended since the turn started plus earlier ones closed since (a resumed fan-out reopens its entries in place); `turn_status`; `turn_result` returns the `run_task` contract's fields (`status, task, summary, files_produced, feature_tables, key_findings, suggested_followups, warnings`) plus `delegations` (count, this turn), `delegation_results`, `verification`, and `error` on failure. Files and findings of a failed delegation are left out. An analysis delegation that ran no analysis is named in `warnings`.
- **`cli/shell`**: `MetaAdapter.headless_run` uses it. `headless.exit_code(result, strict=)`: 0 / 1 as before; under `--strict`, 3 for a `partial` status or for a result whose `verification.all_verified` is not True (judged only when the result carries a `verification`: analyze always, meta when an analysis was delegated, plan and simulate never). `render_result` prints a Verification block.

### Why these choices

- **Status is not changed for "ran no analysis".** The orchestrator cannot know a task was meant to analyse data (a literature question is a legitimate analysis-mode task), so that is `--strict`'s job.
- **`partial` exits 0 without `--strict`.** A retry that worked, or a fan-out with one abandoned branch that still fused, looks the same in the ledger as a real partial failure, and those exited 0 before.
- **The meta gets no `run_task`.** It is the top-level caller (see the MCP handler's note); the reporting is a pure function over the ledger so the MCP server's meta tool can adopt it later.

### Verification

- `tests/test_headless_result_status.py` (new): the summary and merge rules; `run_task` carrying it through the real method; turn status per ledger shape; restored sessions; a fan-out resumed in place through the real `_open_delegation` / `_close_delegation`; chat error; the ledger and checkpoint keeping the verification; exit codes with and without `--strict`; the flag reaching `headless.run`.
- **Live, Bedrock (`bedrock/us.anthropic.claude-opus-4-8`)**, real `scilink ... -p --output-format json`:
  - analyze, synthetic Raman spectrum, `--strict`: `verification {analyses 1, verified 1, all_verified true}`, exit 0;
  - meta, one analysis delegation: the delegation row carries the child's verification through the real ledger, merged `all_verified true`, feature table listed, exit 0;
  - meta, file that does not exist: the specialist replied "file not found", delegation `success` with zero analyses → `warnings: ["Delegation #1 (analysis) ran no analysis."]`, exit 0 without `--strict`, **3 with it**;
  - analyze, missing file, `--strict`: exit 3;
  - meta, invalid model id: `status error`, exit 1 (on `main` this was a reply beginning with "❌" and exit 0).
  - The `partial` path was not produced live; it is covered offline.
- Full offline suite compared against `origin/main` by failing-test ids: 27 failed / 28 errors on both (the same ids; none branch-only), 5027 passed on `main`, 5056 here.
- An independent reviewer agent read the diff before this PR. Its findings are addressed in the third commit: preparation / derivation steps counted as unverified analyses, resumed fan-out entries invisible to the turn, exit 3 without `--strict` being a compatibility break, a mixed turn passing `--strict`, and unattributable merged items.

### Not changed

- `scilink serve`'s `scilink_orchestrate_meta` still reports `error` only for a reply starting with "❌". It can adopt `turn_result`; left out to keep this to the command line.
- `analysis_verdict` for preparation and derivation rows is unchanged (still `verified: False, "no verification record"` on the row); only the summary leaves them out.
- Plan and simulate results carry no `verification`.

### After the review (round 1)

- The turn's delegations are found by change against a snapshot taken after the start-of-turn sweep, not by timestamps: a delegation an earlier, stopped session left running is no longer charged to the next turn.
- `verification` has a `steps` count. A delegation that only prepared data is fine beside one that analysed it (the prescribed shape for raw-instrument data); only a delegation that did nothing counts against `all_verified`.
- An analysis that raises is recorded as a failed row, so it shows under `verification.failed` (and in `list_results`), like one whose agent returned an error.
- Text and markdown output print a status that is not `success`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4160c400f2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.