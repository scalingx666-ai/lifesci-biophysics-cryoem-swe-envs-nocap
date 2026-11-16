# Issue

**Title:** A held pin is logged once; grain_analysis seeds its probabilistic Hough

## Description

## Summary

Two small fixes. . Merged without external review, at the maintainer's request.

- **A unit whose recipe was held logs its pin once.** When a series follower's replay pins a parameter and its locked recipe is kept (#740), the event was logged twice: once in the attempt loop, once after it. The two lines agreed but repeated each other (the #740 review's nit). The post-loop line is now skipped for a held pin.
- **Grain analysis gives the same answer every time (#743).** `straight_boundary_fraction` called skimage's `probabilistic_hough_line` without a seed. The probabilistic Hough transform samples points at random, so the same image gave a curved-boundary fraction of 0.0 or 0.0018 from one call to the next. That could flip whether the twin proxy was reported as reliable, and it made `test_threshold_is_tunable` fail intermittently. It now passes `rng=0`.

## Technical

- **`curve_fitting_controllers._fit_single_spectrum`:** the post-loop "Pinned at bound after relaxation" warning is emitted only when the recipe was not held. Under `hold_recipe`, the attempt loop has already logged "…kept verbatim and the unit flagged (not relaxed)".
- **`skills/_shared/grain_analysis.straight_boundary_fraction`:** `probabilistic_hough_line(..., rng=0)`.
- **Tests:**
  - **The follower-pin test** asserts exactly one "Pinned at bound" record, worded as held. It re-enables logging for its own run and restores the previous level: another module can leave `logging.disable()` set, and the first full-suite run caught exactly that (`caplog` captured nothing).
  - **`test_the_analysis_is_repeatable`:** six calls on one seeded map return identical `curved_boundary_fraction`, `straight_boundary_fraction` and `twin_proxy_reliable`.
  - **Without the changes:** the log assertion fails, and the repeatability test fails in 2 of 3 runs, which is the nondeterminism itself.
- **Full suite** against a `main` worktree: 4023 passed and nothing branch-only. 22 failed / 29 errors, all baseline ids. Two baseline logging-order tests (`test_shell_turn`, `test_ui_stop`) happened to pass this run; that is the known ordering noise, not a fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2af00b8c52f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.