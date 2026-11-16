# Issue

**Title:** A hyperspectral reuse replays one recipe with its own gate, never a merge of a folder's records

## Description

When SciLink re-applies an earlier hyperspectral analysis to new data (a "reuse"), it now picks one method from the earlier run and holds it to the check that method was approved under. Before, a reuse pointed at a series that found several regimes (for example, before and after a new band appears) ran every regime's method at once on the new data. It then checked the result against whichever reference it happened to find first, or against none. That also applied to the copy of the series kept on the swarm board.

The reuse now tries the regimes' methods in the order the series locked them. Each is checked against its own regime's reference, and the first one that passes is kept, as a curve reuse already does. New data from the second regime is therefore read with the second regime's method, not rejected by the first's. Data that neither regime fits is kept with the first regime's method and is not certified. The result says which method was used and what the others gave.

.

---

### Technical details

- **Candidates:** `HyperspectralAnalysisAgent._prior_recipe(prior_analysis_paths)` returns the first recipe and the rest as `alternatives`, each `{records, reference_maps, certification_maps, source, path, regime}`. Per path, in order:
  - **A records file, or a run folder holding one:** one recipe, as before.
  - **A series run folder:** one candidate per regime from `locked_recipes`, via the shared `series_recipes`, in lock order (the anchors' index).
    - Each is the script that regime's followers replayed, never a later refit.
    - Each carries its own map gate (`reference_maps`) and certification reference.
  - **A folder holding several records files** (the board's copies of a series' regimes, a series from before the record, several runs under `results/`): one candidate per file, by the lock order the board recorded, else by name. Each takes its own sidecar gate. The records are never merged.
  - **Several paths:** the first that yields a recipe is used; the rest are named as not replayed. This settles the #749 review note about two regimes' copies held to the first one's ranges.
- **Fall-through:** `_reuse_first_good` runs the candidates through the shared `select_recipe(strategy="first_good")`, as the curve reuse does.
  - Each runs as a child reuse in its own folder (`_candidates/recipe_NN`), with that regime's `reference_maps` as its `replay_reference`.
  - "Good" means the replay came back `success` with a verified verdict: a required output passed that regime's own map gate.
  - The first good one is kept; when none is good, the first that executed is kept and not verified.
  - The kept run's files are copied into the run's own folder, and its paths are rewritten to it, on the parsed values, so a path matches whatever its separators.
  - The tries stay under `_candidates/` for audit. The fan-out's donor probe (`find_donor_reuse_dir`) and the Live tab's reference list (`list_reference_analyses`) skip `_candidates`, so a try is never taken as the run's recipe, least of all a regime this data rejected. The donor probe's `analysis_results.json` loop already skipped it.
  - `script_reuse.source` and `script_reuse.regime_choice` (`chosen`, `chosen_regime`, `tried` with each verdict) record the choice, and `recipe_path` points at the chosen recipe.
  - It engages only when there are alternatives that all carry a map gate, and the caller passed no `replay_reference`. The series driver's own replays and the live loop pass one and keep the first.
- **Board:** a series' recipe record and its copy's sidecar carry the anchor's `index` (the regime's lock order), so a folder of copies resolves in the same order as the run it came from.
- **Unchanged:**
  - a records file;
  - a single run's folder;
  - the series driver's own replays and the live loop, which point at one regime anchor's run folder;
  - the fan-out's harmonized replay, whose donor folder holds one records file;
  - `_load_prior_dynamic_records`, still used by `_locked_targets`.
- **Not decided here:** choosing the regime from the data (`select_recipe(strategy="nearest_first")`, as the curve agent does with its drift distances). As the issue says, that is a separate decision. With fall-through on the gate, a frame of a later regime no longer depends on it.
- **Docs:** CLAUDE.md, series analysis ("A later reuse of a series run replays its locked recipe").
- **Tests** (`tests/test_hs_reuse_recipe_choice.py`, 12). The end-to-end cases run a whole strict replay offline, with no model call.
  - **A two-regime series folder** replays one regime's recipe, never both; `source` names it. On `main` the other regime's map appears too.
  - **Fall-through:** two regimes measure one quantity with one script, and their gates hold different ranges.
    - A frame of regime A keeps A, verified.
    - A frame of regime B falls through to B, verified.
    - A frame of neither keeps A, not verified.
  - **A board folder of copies** replays the first by recorded lock order, held to that copy's gate. Copies with no recorded order are taken by name, the next one an alternative with its own gate.
  - **Several paths:** the first is replayed and the rest named.
  - A records file and a single run are unchanged, and nothing approved is still refused.
  - The board's sidecar records the lock order.
  - After a fall-through, with regime B kept or nothing passing, the donor probe and the Live tab find the run's own recipe or none, never a try.
- **Verification:**
  - Hyperspectral replay, series, records, board, live-loop (incl. the web loop), donor-probe, fan-out and verdict-parity modules: 226 pass. The one failure, `test_retry_ladder_reaches_hot_and_succeeds`, also fails on `main`.
  - Full suite: the same failing tests as `main` (2295d58f), by test id, except the timing-sensitive Ctrl-C test (passes 3/3 alone; the shell is untouched).
- **Live (Bedrock):** six synthetic cubes, with a band that splits in two partway through, analysed as a real series. It locked two regimes, anchored on datasets 0 and 3, with different scripts and map gates. A new cube was then reused, pointed at the series folder:

  | new cube | `main` | first version of this PR | this PR |
  |---|---|---|---|
  | regime A's (320 K) | 6 record sets replayed, both regimes' maps; verified with no gate | regime A; verified | regime A on the first try; verified |
  | regime B's (500 K) | 6 record sets replayed; verified with no gate | regime A only: `partial`, no maps, not verified | regime A rejected by its own gate, then regime B; verified |

  Pointed at the board's copy of the series' recipes, the regime-A cube was replayed with regime A's copy and certified against regime A's reference (on `main`: both regimes replayed, not certified).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2295d58f851b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.