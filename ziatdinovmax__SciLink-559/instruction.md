# Issue

**Title:** Make a fan-out gate decline stick and recover mixed outcomes by retrying only the failed branch

## Description

## Summary

Two things change in a meta session that runs several datasets in parallel.

**Cancel now cancels.** When you decline the "launch this parallel analysis?" prompt, the assistant no longer asks the same question again. Within that turn it will either analyze the datasets one at a time or ask how you want to proceed. Your next message resets this, so "go ahead" later still works.

**A partly failed parallel run is repaired, not repeated.** If most branches finished and one failed, the assistant keeps the finished ones, retries only the failed branch, and then fuses. Asking to re-run the same datasets again is refused with that advice unless you explicitly ask for a fresh run from scratch.

.

## Technical description

### Part 1 — sticky, directive decline (`fanout.py`, `meta_orchestrator.py`)

- A user refusal at `_confirm_fanout` (answer `n`, or no input channel) returns `_declined_by_user_payload`: `status: declined_by_user`, the `fanout_set`, and a directive to not call `delegate_to_analyses` again for these datasets this turn and to analyze independently or end the turn. It deliberately omits the verdict, which was what re-motivated the loop. Machine declines (autonomous verdict, caps) keep their previous payload with the verdict.
- The declined set is recorded on `orch._fanout_declined_sets` (branch ids). `run_fanout` checks it on the requested set before the gate and on the gate-pruned set before confirming, and refuses without prompting. `chat()` clears it at the start of every user turn.

### Part 2 — mixed outcomes (`fanout.py`, `meta_orchestrator_tools.py`)

- `run_fanout` result: `failed_branches` (index, label, data_path, status, timed_out, error head); `next_step` and `warning` direct `resume_fanout(retry_failed=true)` and say not to re-run the whole fan-out. `_productive` is hoisted to `_branch_productive`.
- `resume_fanout(orch, retry_failed=False)`: with the flag, also re-runs the non-productive, non-timed-out branches of the most recent `parallel_group` (interrupted branches still resume as before). A retried entry is stamped `retry_of_error` and `retries`, reopened as running, and run with `_RETRY_NOTE` on its recorded task; the child is restored only when the failed attempt left a `checkpoint.json`. Response reports `branches_resumed` and `branches_retried` separately; the no-op message mentions the flag.
- `_already_ran_decline`: `delegate_to_analyses` over a dataset set (keyed by `(data_path, pattern)` pairs, so relabeling or rewording does not evade it) that already ran as a completed group with at least one productive branch is refused with the group id, productive indices, failed labels, and the fuse-then-retry directive; checked before the gate and again on the pruned set. Not applied when the group is unfinished (resume's job) or when nothing was productive (a full re-run is legitimate). `force_rerun=true` overrides.
- Tool surface: `delegate_to_analyses` gains `force_rerun`; `resume_fanout` gains `retry_failed`; both descriptions say when to use them. Two sentences in the meta prompt state the two principles.

### Tests

`tests/test_fanout_decline_and_retry.py` (15 cases): decline payload has no verdict and a directive; same set refused without prompting, including relabeled tasks; reset at turn start; EOF counts as decline; confirm records nothing; autonomous decline unchanged; mixed result names failed branches; already-ran refusal with indices and directive, no gate call, `force_rerun` re-runs; guard applies to the gate-pruned set; guard needs a productive branch; a different set is not refused; `retry_failed` retries only the failed branch of the latest group with the retry note and no restore when no checkpoint; restore when a checkpoint exists; timed-out and older groups untouched; plain resume unchanged; tool schemas expose the flags. Existing resume, steering, panel-filter, donor-probe and cancellation suites pass. `test_meta_fanout_robustness.py` shows the same two timing checks failing on main as here (pre-existing, unrelated).

### Live verification (Bedrock `us.anthropic.claude-opus-4-8`, real meta and gate LLM, fake branch children)

- AUTOPILOT with the confirm answered `n`: one prompt, one `delegate_to_analyses` call. The model stated the user had declined the parallel run and switched to two independent delegations plus a fusion.
- AUTONOMOUS with the spectrum branch failing on its first attempt: one fan-out, then `resume_fanout(retry_failed=true)`, then fusion; the ledger shows the branch retried once and successful.
- An explicit "run it again from scratch" on the same session: the model passed `force_rerun=true`, the override working as intended.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1af391f4e6f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.