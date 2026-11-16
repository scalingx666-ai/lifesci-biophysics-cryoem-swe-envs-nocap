# Issue

**Title:** Live: Stop ends the frame that is running

## Description

## Summary

Pressing Stop in the Live tab used to stop the run only after the frame being analysed had finished, which could take up to the script time limit. Now Stop ends the running frame at once. That frame is recorded as "interrupted", not as a failed fit, so it doesn't count against the recipe.

## Technical description

**The gap.** `LiveRun.stop()` (and a "stop" decision at a pause) only set `_stop`. The loop checks it between frames, so a replay already running (warm worker or cold subprocess) ran to its end or to the executor timeout. The chat Stop already reaches running scripts through `executors.kill_subprocesses_for_thread`; the Live tab never called it.

**The change.**
- **`MeasurementLoop.step`** records the thread analysing the current frame (`_frame_thread`).
- **`MeasurementLoop.interrupt()`**, callable from any thread, calls `kill_subprocesses_for_thread` on that thread. That ends the replay script and, with #684, everything the script started. It returns whether a frame was running.
- **The interrupted frame** is logged with the single flag `interrupted` (`FLAG_INTERRUPTED`). It skips the gates, the drift monitor's learning, escalation and the recommender, so it neither counts as a failed fit nor teaches the monitor.
- **Callers:** `LiveRun.stop()` and a stop decision at a pause both call it.
- **Placement:** it lives in `scilink.live`, not the server, so a library caller of `run_experiment` can call it too, keeping the live layer free of session code.

**Tests: `tests/test_loop_interrupt.py`.**
- A replay running a real 60 s script through a `ScriptExecutor`, with warm and cold executors, is interrupted from another thread. The frame ends within 10 s, logged as `interrupted`, with `_consecutive_breaches` still 0.
- `interrupt()` with no frame running is a no-op.

The existing loop and Live-tab suites pass (193 tests).

**Live verification.** The replay path makes no model call by design (strict replay), so a live Bedrock run adds nothing the real-subprocess tests do not cover. In the combined integration run with every open PR merged, a 15-frame live loop on Bedrock (simulated in-situ Raman) behaved as before: no flags, at most 1.1 s per frame.

- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `739d5c17891b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.