# Issue

**Title:** A reuse replays the series' locked recipe, and a script file is a recipe

## Description

## Summary

Two small questions the stage-2 review left for stage 3 of the swarms, answered together because they are one question — *what does a later reuse replay?*

- **A series' recipe is the script its table rests on** (#704). When you later reuse a series run on a new measurement, the replay is now the recipe the series locked and its followers ran — not the script a later refit left in the anchor's slot. The refit's script stays in the run (`scripts/<unit>.py`); a person who wants that better model starts a new series from it. The result says which script was replayed and whether the anchor had been refit since (`reuse_validity.source`).
- **A script file is a recipe** (#705). `prior_analysis_paths` can name a `.py` file (or, for a datacube, a `dynamic_analysis_records.json`, which already worked) instead of a run folder: the file is replayed through the same gate and gets the same `reuse_validity` verdict. This is what lets the swarm board's own copy of a recipe (`swarm/recipes/…`) be replayed by a later item.

Nothing changes for a single-spectrum or single-image run, or for a series run from before the recipe record (both keep today's pick).

## Technical description

- `_verification_record.prior_recipe_script(anchor_dir, *, single_name, named=None)` is the one pick, used by `_load_prior_curve_fit_state` (curve) and `_first_prior_image_script` (image): the named `.py` → the run's own single script → the series' locked recipe (`locked_recipes` in the run's `analysis_results.json`, first regime by index; the label says "the anchor refit since" when `scripts/<unit>.py` differs from it) → the first unit script (pre-record runs). `named_recipe_file(raw_path)` recognises a `.py` path. A `.py` on its own (no run folder) returns `(parent, None, text, label)` from the curve loader, so the realtime path, which needs a run, still refuses it.
- The source label (`reuse_validity.source`) is the run name as before for a single run; for a series or a named file it appends which script was picked and why.
- `run_analysis`'s `prior_analysis_paths` description documents the file form and the series rule.
- Tests: `tests/test_series_verdict_path.py` (M1/M2 case: the run replays M1 and says so; the named refit file replays M2; the board's copy replays M1 with no run folder; a pre-record run as before) and `tests/test_image_series_verdict_path.py` (two regimes: the first regime's recipe; the anchor's script changed after the lock; a named file inside the run and the board's copy; a pre-record run; a single run unchanged).
- Verification: the two harnesses (18), the reuse-related suites (170: harmonize donor, HS locked replay, measurement loops, script edits, prior-run discovery, resume fan-out, …), full suite against the `main` baseline: 24 failed / 29 errors, the same ids as `main`; no branch-only failure (3886 passed).
- Stage 3 of the swarms is branched on top of this and exercises the file form live (a verified recipe fired as a replay on a new spectrum).

### After the review (round 1)
A script file is a recipe and nothing more: the loader returns no `anchor_dir` for a bare `.py`, so the realtime profile and the live loop refuse it as on `main`, while a reuse replays it; a file inside a run names the run for the loop, which arms on the script the run replays. A curve reuse of a series that locked several regimes replays the recipes in lock order and keeps the first the R² gate calls good (`reuse_validity.recipes_tried`); images replay the first regime's and the label says so. The refit check uses the agents' saved file name; an unverified recipe says so in its label; `series_recipes` is typed; an image strict replay with no reference run is refused; the two stale "any one is the reusable fit script" hints are corrected. Full suite: no branch-only failure.

### After the review (round 2)
Several regime recipes never share a working dir: each runs in `_candidates/recipe_NN` (the best-of-N layout) and the kept one is promoted, so the figure and `fit.npy` on disk are the kept result's whichever candidate ran last. The recipes run verbatim first; only when none executed does the first get the correction ladder, once. Under strict replay a raising recipe moves on to the next regime. The live loop refuses a series anchor that locked several regimes (it locks one recipe). Real-path tests with a script-keyed executor cover each case. Full suite: no branch-only failure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c188d014bd52`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.