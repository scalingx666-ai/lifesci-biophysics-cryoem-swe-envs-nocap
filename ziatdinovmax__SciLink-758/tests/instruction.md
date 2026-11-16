# Issue

**Title:** A replay of the board's copy of a recipe is certified like a replay of the run

## Description

## What this changes

When a swarm item finishes an analysis, the swarm board keeps its own copy of the recipe (the fitting script) so a later item can reuse it. A reuse of a recipe is checked twice before its claims count as verified. First, did the recipe find what the original run found? Second, does the new data look like the data the recipe was built on?

Both checks need the original run's data and results to compare against. The board's copy had only the script, so a replay of the copy could never be checked. Its claims stayed provisional forever, and nothing said why.

Now the agent records a small **certification reference** where it records the recipe. For a spectrum this is what the units found and a compact summary of their curves; for a datacube it is the reference maps' statistics. The board copies that reference next to the script. A replay of the copy is then checked exactly as a replay of the original run would be: it is certified when the new data matches, and flagged when it does not. The reference can only withhold a certificate. It never changes the replay's verdict, so a copy passes or fails exactly where its run would.

When a replay is not certified, the result and the board now say why: no reference travelled with the recipe, the recipe found different features, the new data differs from the regime's, or (for images) the modality has no interpretation check yet. Before, a replay came back unchecked with no reason.

Nothing changes for analyses outside the swarm. A reuse of a run folder already had its data, and still uses it first.

---

## Technical details

- `_replay.curve_certification_reference(drift_state, x_range, samples)` builds `{"kind": "curve", "drift_state", "x_range", "identity"}`. The shared reasons are `NO_REFERENCE` and `NO_INTERPRETATION_CHECK`, with `no_interpretation_check()` for image reuse.
- Curve:
  - `_restamp_regimes` stamps each regime's `certification_reference` from its own units' curves (the drift state) and parameters (identity samples);
  - a single run's results carry the reference of its one locked recipe;
  - `_regime_references` falls back to the carried reference when there is no run data;
  - `_identity_of` names `NO_REFERENCE` when nothing can be checked.
- Hyperspectral:
  - the series lock and `_write_results_file` stamp `{"kind": "maps", "reference_maps"}` (not on a live frame);
  - a series copy is held to the map gate its recipe was approved under (#734), unchanged;
  - with no map gate (a single cube's copy, or its run folder), `_recipe_certification_maps` reads the reference, and the controller sets `identity_checked` from `map_health` against it, **certification only**: `replay_reference` stays empty, so the verdict is the run's (#711).
- Image: the four `reuse_validity` sites carry `certification: no_interpretation_check()`.
- `_verification_record`: `series_recipes` carries the key and `recipe_sidecar` passes it through. `prior_recipe_candidates` returns it for a named file, a single run and a series.
- Board: the reference travels on a private spec key and is written into the copy's `<stem>.recipe.json` sidecar only. **It never goes into a board record**, which stays small (a drift state is kilobytes).
- Why a replay is not certified: `_replay.certification_reason` reads it from `reuse_validity`. `replay_certification_reason` puts it on the `analyses` row (beside `escalation`), and the board's provisional claim quotes it.
- Tests (`tests/test_board_copy_certification.py`, through `post_delegation` and the real `qc_try_reuse`):
  - the sidecar holds the reference and the board does not;
  - a replay of the copy is certified on its own regime and flagged on another;
  - a copy without a reference names `NO_REFERENCE`;
  - single-run curve and cube copies carry it, and a cube's copy and its run folder certify from the same maps;
  - a certification reference never decides a cube replay's verdict: the same maps as a gate reject an out-of-range map, and as a certification reference they only withhold the certificate (a real locked replay, zero model calls);
  - `_restamp_regimes` stamps each regime from its own units;
  - the board quotes the reason on a provisional claim.
  - `tests/qc_golden/test_certification_stamp.py`: through the real curve writer, a thorough run stamps its reference and a realtime frame replaying it does not.
  The image strict-replay golden test asserts the reason.
- CLAUDE.md: one sentence in the board section.

**Live check (Bedrock, real data).** A two-regime curve series was run with two units held out. Its recipes were posted to a real board, and each board copy was replayed on each held-out unit (four replays). Before this change, none of the four could be checked.
- Same regime, multi-unit reference: **certified** (identity within, state distance 0.00).
- Each cross-regime replay passed its fit gate (R² good), yet was **flagged** on identity (missing or new strong positions) and not certified. One was also flagged on state (0.78). Both were escalated to the judge.
- Same regime, one-unit reference: not certified. This is the existing one-reference rule (#725): with no spread known, a one-sample shift withholds the certificate. The judge read it as the same regime and the same interpretation, recorded as a provisional opinion.

Reference size in a sidecar: about 27 KB for a one-unit regime and 62 KB for a seven-unit one. The board file was unchanged in size.

A live frame (the realtime profile, or a strict replay) stamps no reference, for curves and cubes alike; it is not a recipe source.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55b86f181ec8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.