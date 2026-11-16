# Issue

**Title:** A turn's Stop adds to the thread's cancel; CIs and RMS name another quantity

## Description

## What this changes

Three small fixes left as non-blocking notes in the reviews of #759 and #760.

- **Stopping a turn no longer silences another cancel.** When a chat turn starts, the user's Stop button is wired to the thread's cancel. If that thread already had a cancel of its own, for example a worker's time budget or memory limit, the Stop used to replace it for the length of the turn, and the worker's own cancel went unheard until the turn ended. Now either one ends a wait. No current entry point runs a turn on such a thread, so this closes a gap before anything can fall into it.
- **`CIs_` and `RMS_` columns are no longer taken for the measured quantity.** When a series lines up its feature names across datasets, a column whose prefix names an error or a noise level must never be matched to the measured column. `CIs_Area` (confidence intervals) and `Rms_Depth` / `RMS_Depth` used to be matched to `Area` and `Depth`. Now they are not. `Cis_Isomer_Area` (a cis isomer) still matches `Isomer_Area`, as before.

---

## Technical details

- **`log_context.register_turn_stop`:** when the thread already carries a cancel, it registers `_AnyEvent(prev, turn_stop)`.
  - `_AnyEvent` is a `threading.Event` whose `is_set()` is true when any member is set, and whose `wait()` polls in 50 ms slices.
  - It still returns the previous cancel, and `restore_cancel` puts that back.
  - A thread with no prior cancel gets the turn's event itself, exactly as before.
  - Readers (`cancel_requested`, `raise_if_cancelled`, `inherited_context`, `attributed_to_current`) only call `is_set()` or hand the event on.
- **`hyperspectral_series`:**
  - `_words` keeps the original case, and `_same_quantity` lower-cases where it compares.
  - `_is_quantity_word` also folds a plural below the four-letter floor, but only for an upper-case stem with a lower-case `s` (`CIs`, `SNRs`).
  - `rms` joins `_QUANTITY_PREFIX_WORDS`.
- **CLAUDE.md:** one sentence beside "A cancel must reach a waiting worker".
- **Tests:**
  - `test_a_capture_adds_the_turns_stop_to_the_cancel_it_found`: a thread's own cancel, set mid-turn, raises and ends a `wait()`, and the original comes back afterwards.
  - `test_a_capture_restores_the_cancel_it_found` now asserts the turn's Stop reaches the cancel instead of pinning the replacement.
  - `test_units_drift_only_on_one_side_and_quantity_words_are_whole_words` adds `CIs_Area`, `SNRs_Map`, `Rms_Depth`, `RMS_Depth` and `rms_depth` as gaps, with `Cis_Isomer_Area` still aliasing.
  - The new assertions fail on `main`.

**Measured.**
- **The `rms` word:** in 688 real feature tables on this machine, every column with `rms` as a word is a residual, noise or diagnostic quantity.
- **All pairs:** across all 2.1 million ordered column pairs within those tables, `_same_quantity` gives the same verdict on `main` and here. Nothing that aliases today is blocked.

**Suites.**
- The Stop, cancel, swarm, human-input, executor and series suites pass, except four `test_log_context_attribution.py` tests.
- Those four fail in this file combination on `main` as well (an order dependency between test files). They pass when the file is run alone, on both.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dee13701e38`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.