# Issue

**Title:** Make a prior run's locked scripts discoverable

## Description

.

## Summary

A series curve fit keeps its locked fitting script under `scripts/` in the output directory, one copy per spectrum, but an agent asked to re-fit a spectrum with that model could not find it: the results manifest pointed only at the trend script, examining the directory listed the data files, and reading the `scripts/` folder returned "Not a file". The agent concluded the script lived elsewhere and re-derived a new fit. Now all three routes reveal the scripts: reading a directory lists its contents and names saved scripts, the manifest records the scripts and how to reuse them, and examining a prior output directory says so and lists them.

## Technical description

- `scilink/utils/file_io.py`: `read_file_content` on a directory returns `list_directory(path)` — `is_directory`, `entries` (sub-directories with counts, files with sizes, capped at 300), `scripts` (top-level and `scripts/*.py`), and a hint. All four orchestrators' `read_file` tools share this reader.
- `CurveFittingAgent._save_fitting_scripts` returns the saved paths; the manifest gains `fitting_scripts` (`dir`, `files`, `representative`, `note` with the `prior_analysis_paths` + `reuse_locked_script` / `script_edits` route). `ImageAnalysisAgent` records `analysis_scripts` the same way.
- `examine_data`: `_analysis_output_dir_summary(path)` adds `analysis_output_dir`, `analysis_results`, `saved_scripts` and `prior_run_hint` when the directory holds a manifest or a `scripts/` folder; plain data directories are untouched.
- Tests (`tests/test_prior_run_script_discovery.py`, 6): directory listing and its cap, the manifest record for series and single runs, examine_data on an output directory and on a plain data directory. File-tool suites: 22 pass.

### Live check (Bedrock)

A real four-spectrum series fit produced `scripts/spectrum_0000..0003.py` and the new manifest record. A real analysis-orchestrator turn was then asked to re-fit index 2 reusing the locked script with one bound relaxed. Its tool trace: `examine_data` on the prior directory returned `analysis_output_dir: true` with the four scripts; `read_file` on `scripts/spectrum_0002.py`; `run_analysis` with `prior_analysis_paths`, `reuse_locked_script: true` and a `script_edits` pair changing the amplitude ceiling to `10.0 * max(np.max(y), 0.1)`; the final call returned `reuse_validity`. The agent never re-derived a model.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da6cc29dfdca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.