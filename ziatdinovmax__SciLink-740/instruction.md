# Issue

**Title:** A series follower whose replay pins a parameter keeps the locked recipe and is withheld as degenerate (#726 follow-up)

## Description

## Summary

 (closed by #738), on its noted side effect, as the #738 review recommended.

**What went wrong.** In a series, every unit after the anchor replays the anchor's locked recipe, so all units are fitted the same way and their numbers are comparable. When a follower's replay ended with a parameter stuck at its bound, the pinned-at-bound rule (#592) sent it through the correction ladder. The ladder *rewrote that unit's bounds* and refitted. In the #726 run every follower did this, so each unit's secondary components came from different bounds and the columns of the feature table no longer meant the same thing from row to row.

**What changes.**
- **The recipe is kept.** A follower whose replay pins a parameter now keeps the locked recipe exactly as it is (`replay_verbatim: true`).
- **The pin is flagged, not re-derived.** It is flagged on the unit (`pinned_at_bound`, with the parameter and its bound), the same way a series flags any anomaly rather than re-deriving it. Pinned rows are not a refit reason, so the unit is not re-analysed.
- **The unit's verdict reads its own pin.** It is withheld as a degenerate fit, for example "replayed the locked recipe of unit spectrum_0000, but this unit's fit is degenerate: peak_1.fwhm = 12 at its upper bound 12". Before, a follower's verdict looked only at the recipe it replayed, so its own pin was invisible there.
- **Unchanged:** a follower whose script fails to *run* is still repaired. Anchors, refits, prior-recipe reuses and the script bank's edit-adapt still relax a pinned bound as before; this applies only to series followers replaying their regime's recipe.

**Is anything tuned to data?** No. The rule depends only on whether the unit is a follower and whether its fit reports a parameter at its bound.

## Technical

- **`_fit_single_spectrum(..., hold_recipe=False)`.** With `hold_recipe`, a pinned result leaves the attempt loop as it is. There is no `PINNED_BOUND_FIX` retry; the warning says the recipe was kept and the unit flagged. The existing post-loop code stamps `pinned_at_bound` and the "Degenerate fit" `quality_warning`, and `_detect_outliers` flags it `pinned_at_bound`, which is not in `AdaptiveRefitController.REFIT_REASONS`.
- **Both follower call sites** (the serial path in `UnifiedSeriesProcessingController.execute` and its deferred parallel path `_run_deferred`) pass `hold_recipe=base_script is not None`. The other callers that pass a script (`_run_reuse_candidate`, a prior-recipe reuse whose ladder-repaired replay counts as a replay by design, #717; `_try_bank_edit_adapt`, an adaptation) are unchanged.
- **`_verification_record.unit_verdict_for`**, follower branch: when the recipe verified but the unit itself carries `pinned_at_bound` (top level or under `fit_quality`), the verdict is `verified: false`, `decided_by: recipe`, with the reason naming the pin. A recipe that was not approved keeps its own reason first.
- **CLAUDE.md:** the series paragraph says it.

**Tests:**
- `tests/test_series_verdict_path.py::test_a_follower_whose_replay_pins_keeps_the_recipe_and_is_named_degenerate` runs the real series driver. One follower's replay reports `fwhm` at its upper bound, with the bounds printed as a real script prints them. The test checks that:
  - the follower is executed once, never relaxed;
  - its row is `locked_script`, `replay_verbatim: true`, with the pin;
  - it is flagged `pinned_at_bound` and not refit;
  - its verdict names the degenerate fit;
  - the other follower verifies;
  - the series is withheld, naming the unit.

  It fails without the change.
- Neighbouring suites (series verdict path, verdict parity, replay escalation, reuse gate, regime choice, refit budget, board): 112 passed.
- Full suite against a `main` worktree: 4016 passed (+1), 24 failed / 29 errors on the baseline ids, nothing branch-only.

**Live:** not exercised. A series with followers whose shoulder is five times broader than the anchor's was run to provoke a pin, on Bedrock `us.anthropic.claude-opus-4-8`. The scout correctly split it into two regimes, each anchored with its own bounds, so no follower pinned. The series verified, with its follower replayed verbatim. That is evidence the change doesn't disturb a normal series, not a live test of the pin rule; the real-path test above is the test.

## After the review (round 1)

- **The held path says what happened, in four places:** the log line, the stored `quality_warning`, the `script_errors` diagnosis and the flag's recommendation. All four use one text (`PINNED_HELD_NOTE`): the locked recipe was kept for comparability, the value at its bound is not a measurement of this unit, and a separate analysis or a new series anchored on this regime is needed for it. None says "relax" or "refit" any more.
- **The unit records `pin_held`,** which the outlier pass reads.
- **The non-held recommendation** drops its amplitude-only "2 × max(y)" example.
- **The test** checks all three stored texts.
- **Suite:** 4016 passed, the same baseline ids.
- **The reviewer's live checks** (a forced pin, real in-situ FTIR, end to end with #741) showed the rule firing as intended. The cascade from a degenerate *anchor* is #742.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6854e38b1447`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.