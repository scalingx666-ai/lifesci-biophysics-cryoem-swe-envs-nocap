# Issue

**Title:** Recognise image and curve donors in harmonized fan-out replay

## Description

## Summary

When the meta agent runs a **harmonized fan-out** (analyze one dataset first, then re-run its approved script unchanged on the sibling datasets so the numbers are directly comparable), it only knew how to recognize the approved script of the **hyperspectral** agent. If the first dataset was an image or a 1D curve, the fan-out silently gave up on harmonizing: every sibling was analyzed from scratch with its own freshly written script, results were not method-comparable, and slow image analyses could run into the per-branch time limit and be dropped.

This PR teaches the fan-out to recognize the approved scripts of the **image and curve agents** too. Those agents already had the "replay this exact script" ability; the fan-out just never found the script to hand them.

Observed on a real three-map interferometry fan-out: before, the donor finished but both siblings fell back to independent analysis and were cancelled at the 60-minute budget. After, both siblings replayed the donor's script verbatim (reuse verdict "good"), the whole fan-out took 39 minutes, and all three carry the harmonized stamp the fusion step uses.

---

## Technical details

- `scilink/agents/meta_agent/fanout.py`: new `find_donor_reuse_dir(donor_dir) -> (reuse_dir, kind)` replaces the inline `dynamic_analysis_records.json` search.
  - `kind="records"`: hyperspectral records with `script` + `task_success` (unchanged logic, still tried first).
  - `kind="script"`: newest `analysis_results.json` with status `success`/`partial` whose sibling `scripts/` holds `analysis_script.py`, `fitting_script.py`, or per-item series scripts, which is exactly what `_first_prior_image_script` / the curve prior-script lookup resolve on the follower side via `prior_analysis_paths` + `reuse_locked_script`.
  - Guards: a run directory that has a records file is judged by its records only (rejected hyperspectral records are never revived through `scripts/`); `_candidates/` best-of-N folders are skipped.
  - `harmonized_info` gains `reuse_kind`; the log line names the matched kind.
- No changes to the image, curve, or hyperspectral agents.
- Tests: `tests/test_fanout_donor_probe.py` (10 offline cases: records-first, image/curve/series scripts, failed run, no script, candidates ignored, records-authoritative guard, newest wins). Existing suites run green: fan-out robustness, resume, cancellation, partial-donor harmonize, hyperspectral locked replay, image script-edit reuse.
- Pre-existing, unrelated failure on `main`: `tests/test_hs_dynamic_records.py::test_retry_ladder_reaches_hot_and_succeeds` (annealing-ladder expectation), reproduced on an untouched checkout.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `faf3423aea37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.