# Issue

**Title:** The optimization hand-off skips unverified series units, names them and records them

## Description

## Summary

.

**What went wrong.** Since #735, every unit of a series appears in `features.csv`, with a `verified` column saying whether its analysis passed its quality gate. The planning agent's ingestion, which turns that table into the optimizer's training data, never read the column. A **salvaged** unit, whose numbers passed no gate (the #722 run's were positions stuck at a window edge and depths of about 10⁻¹²), entered a campaign as an ordinary measurement with full weight.

**What changes.**
- **Skipped by default.** When the table has a `verified` column, rows marked unverified are left out of what the extraction reads, and the result names each one with its reason ("film_dose20 (unverified)", "film_dose60 (analysis_failed)").
- **Recorded on the campaign.** The skipped units are kept on the campaign's record of that file, so a later refinement knows which measured conditions were not used.
- **Opt-in.** `include_unverified=True` ingests the salvaged units on purpose, as low-confidence data, and records that.
- **Everything unverified:** the ingestion stops with that reason and the hint.
- **Unchanged:** a table without the column (not a series, or written before #735) ingests exactly as before.

**Live** (Bedrock `us.anthropic.claude-opus-4-8`): a planning agent was asked to load a six-unit dose-series feature table as optimization data. The table had one salvaged unit, carrying a wild value (0.95 among 0.1–0.5), and one failed unit.
- **The call:** the model called `analyze_file` *without* `include_unverified`.
- **The result:** 4 data points, with the two units skipped and named.
- **What it told the user:** "Units left out: yes, 2 were skipped — `film_dose20` unverified, its analysis passed no quality gate; `film_dose60` analysis_failed … These two rows were not used to train the optimizer".

**Data-independent:** the rule reads only the `verified` flag that the analysis side writes.

## Technical

- **`OrchestratorTools._verified_rows_only(file_path, include_unverified)`.** For a `.csv` with a `verified` column, it writes the verified rows to `<session>/ingest/<parent>/<same file name>`, with the file's sidecar copied beside it so the scalarizer reads the same metadata. It returns that path plus the skipped units (`unit`, `flag_reason` or "unverified"). It returns `None` when every row is unverified, and the original path in every other case. The flag is parsed tolerantly: `False` / `false` / `0` / `no` are unverified, and an empty cell (unknown) is kept.
- **`analyze_file`:**
  - Both `scalarize` calls read that path, so table pass-through and code generation filter alike, as the issue asked. The file hash, dedup tracking and sidecar merge stay on the original file.
  - The `analyzed_files` entry (the persisted campaign record) gains `skipped_unverified` or `included_unverified`. The latter lists only the units actually ingested; a failed unit has no values and is still dropped by the #534 missing-value skip.
  - The result gains `rows_skipped_unverified`, `rows_skipped_unverified_units` (or `included_unverified_units`) and an `unverified_warning`.
  - The tool schema documents `include_unverified` (default false, "only when the user deliberately wants them").
- **Guidance:** the meta's ANALYSIS RESULTS → PLANNING / BO guidance and CLAUDE.md's `features.csv` sentence say it.

**Tests** (`tests/test_ingest_unverified.py`, real path: a series result written by the real `write_feature_table`, the real `analyze_file`, the real scalarizer pass-through, with no model call):
- three verified, one salvaged and one failed unit: three rows ingested (the optimizer's data holds doses 10, 30, 40), both others named with their reasons, and the record persisted;
- `include_unverified=True`: the salvaged row is ingested and recorded as included, and the failed one is skipped as missing and named;
- a table without the column is unchanged;
- an all-unverified table is refused with the reason.

The first three fail without the change; the fourth passes without it and guards that a table without the column is unchanged. The tests set the sandbox approval per test: `test_scalarizer_sandbox.py` pops `UNSAFE_EXECUTION_OK` from the process environment without restoring it, so a module-level setting failed when the two ran together, depending on order.

The neighbouring suites (#535 continuation, #534 missing values, scalarizer pass-through, fidelity, sandbox and review, BO seed) all pass (32).

Full suite against a `main` worktree: 4019 passed (+4), 24 failed / 29 errors on the baseline ids, nothing branch-only.

## After the review (round 1)

- **`include_unverified` says what it does:** the campaign record lists the units as unverified, and the optimizer weights them like any other point. It no longer says "low-confidence".
- **The copy extraction reads is named as derived:** `features.verified_only.csv`, with its sidecar copied under the same stem.
- **Suite:** 4018 passed. The one branch-only failure is a pre-existing nondeterminism in `grain_analysis` (#743), unrelated to this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6854e38b1447`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.