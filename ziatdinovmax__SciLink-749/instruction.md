# Issue

**Title:** A hyperspectral series records locked_recipes and posts one recipe per regime

## Description

## Summary

.

**What went wrong.** A hyperspectral series never posted its recipe to the swarm board, even when every unit verified. The board reads a series' recipes from `locked_recipes`, which only the curve and image series drivers wrote. A later swarm item therefore had no verified hyperspectral recipe to build on, and the gate it was approved under was lost.

**What changes.**
- **The recipe is recorded where it locks.** The hyperspectral series driver records each regime's recipe at that point, in the curve and image drivers' shape: the anchor unit, its verdict then, and the recipe its replays ran. A cube's recipe is its `dynamic_analysis_records.json`, a records file rather than a script. The record also carries the map gate's reference ranges that its replays were held to.
- **One copy per regime.** The board posts one recipe per regime, copying each under its unit's own folder (`…/<analysis_id>/<unit>/dynamic_analysis_records.json`). The name is kept because it is the name `prior_analysis_paths` reads a cube recipe by; the folder keeps two regimes' files from colliding.
- **Single cubes are unchanged.**

---

## Technical details

- **`HyperspectralAnalysisAgent._analyze_series`:** at `locks[rname] = lock` it writes `locked_recipes[rname]` with `unit`, `index`, `regime`, `file`, `script` (the records file's text), `verdict` (the unit stamp), `model` (the targets) and `gate` (`{"kind": "map_health", "reference_maps": …}`). The value is carried in `state` and on the series result.
- **`_verification_record.series_recipes`** passes `file` through.
- **`board._recipe_specs`:** a recipe with a `file` keeps that name and adds `_subdir: <unit>`; a script stays `<unit>.py`. `_materialize_recipe` writes into the subfolder.
- **CLAUDE.md:** the board rule.

**Tests** (`tests/test_hs_series_recipes.py`, 2, through the real hyperspectral series driver with the stubbed single-cube pipeline of `test_hs_series.py`, then the real board):
- a two-regime series records both recipes where they lock, with the records text, the verdict and the gate reference;
- `post_delegation` posts two verified recipe records with distinct copies, each named `dynamic_analysis_records.json`, and the agent's records loader (the reuse path's) reads each copy.

Full suite against a `main` worktree: no branch-only failures.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff620304bd3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.