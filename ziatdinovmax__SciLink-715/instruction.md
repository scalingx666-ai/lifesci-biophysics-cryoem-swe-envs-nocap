# Issue

**Title:** Hyperspectral scripts get more time when they are merely slow

## Description

## Summary

Split out of #714 at the reviewer's suggestion so it can merge on its own. Spectra and images already retried a timed-out script with a doubled limit before calling it broken; hyperspectral ran every script under a fixed 10 minutes and sent a slow-but-correct per-pixel fit back to the model to be "corrected". Hyperspectral now runs under the same policy — within the run's deadline, never on the fast clock — the record says what limit the script needed, and a replay of it starts from there.

## Technical description

- `_locked_exec.escalate_timeouts(attempt, base_timeout, timed_out, remaining_s=, escalations=)` is the policy on its own (same script, `TIMEOUT_GROWTH`×, `TIMEOUT_ESCALATIONS` times, `TIMEOUT_HARD_CAP_S`; the RETRIES clamped to what is left on the run's deadline and the verification loop's budget — the first limit never, a started script finishes; `escalations=0` for a strict replay); `stage_and_run_adaptive` is a call of it (byte-identical behaviour for curve and image).
- The hyperspectral controller's in-process `exec` + `_invoke_analyze_feature` run under it with the sandbox's own limit as the timed-out outcome — `SandboxTimeout`, one `TimeoutError` subclass raised by the SIGALRM handler and injected into a worker thread alike, tested by type (nearly every interactive surface runs off the main thread); a script's own `TimeoutError` is an error for the repair loop — raised as before (ladder currency) once the budget is spent. Any failed attempt drops its exec scopes — a timed-out one its traceback, an error its traceback's frame locals (`clear_frames`; the prompt's traceback is unchanged) — before the retry or the repair call, so peak memory is not doubled. A script's other exceptions are re-raised in the controller frame without the policy's frames, so the correction prompt's traceback is the script's. `timeout_used_s` is reset per attempt and recorded on the dynamic record; a replay target carries the donor's and a locked replay starts from it.
- Round-1 items of #714 on this part, all in: the traceback freed on timeout; limits clamped to the remaining deadline; no escalation under `_strict_replay`; `timeout_used_s` per attempt; the script's own `TimeoutError` distinguished; the stale comment rewritten.
- Tests (`tests/test_hs_timeout_escalation.py`): the policy's cases (growth, cap, the deadline clamp, no escalation); through the real dynamic-analysis controller on a supplied-script replay with no model call: a script sleeping past a 1 s limit times out at 1 s and succeeds at 2 s, the record says 2 and a replay of it runs once at 2; a strict replay fails at 1 s with no escalation; a 1.5 s deadline is never run past; a script's own `TimeoutError` is not escalated.
- The four hyperspectral goldens regenerated for the additive `timeout_used_s` (and the correction traceback's top line, which now reads the re-raise); the six prompt-drifted goldens fail on `main` as before.
- Full suite against the `main` baseline (post-#717): the same ids, no branch-only failure (3983 passed).
- Not here, as the review notes: hard-killing C-bound hyperspectral code needs the subprocess executors and a change of the script contract — its own issue.

### After the review (round 1, ff85142a)
Rebased onto `main` after #717. One timeout type on both threads (`SandboxTimeout`; the message test missed every worker-thread surface); an error's scopes cleared and the frame's references dropped at the re-raise (a weakref probed inside the repair call); the first limit never clamped, only the retries, which also read the verification loop's own budget (`ctx.loop_started` at `run_item`); the log says what stopped the escalation.

### After the review (round 2, 51239bd7)
The error path's traceback frames are cleared (the frame chain reached the policy's frame whose `out` is the exception — a cycle only the GC breaks; the test now probes with the GC disabled, on both threads). The fan-out stamp asked for in round 1 was dead code and the ask withdrawn: removed, with a behavioural test pinning `main` (a child's `run_task` carries no `time_budget_s`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1fb449b31fe1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.