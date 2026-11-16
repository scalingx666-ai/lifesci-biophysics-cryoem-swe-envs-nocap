# Issue

**Title:** Swarm items get the fan-out's time budgets; a fan-out branch keeps its own depth; a re-run of a timed-out script starts where it stopped

## Description

## What this changes

Three small fixes to how long work is allowed to run and with what settings when the meta agent runs several jobs at once.

- **A swarm job gets the same time a fan-out job gets (#700).** A fan-out already gives heavy analyses more time: twice the default for a series of datacubes, three times for a raw instrument container. A swarm gave every job a flat hour, so the same hyperspectral series was cancelled at half the time as a swarm job. Swarm analysis jobs now use the fan-out's rule. A time limit the caller sets is still taken as given.
- **A fan-out branch keeps the settings it was given (#718).** A branch can ask for its own depth (`profile`), the quantities it needs (`targets`) or a time budget. These were dropped on the way to the analysis, so every branch ran on defaults. They now reach the analysis, are recorded, and a retried branch runs under them again.
- **A slow script is not re-timed from scratch on every retry (#719).** When a hyperspectral replay timed out and the retry ran the very same script, the timeout escalation started again from the bottom (1 s, 2 s, 4 s, every time). A retry of the same script now starts from the limit it already reached. A log line that said "1 s left" when nothing was left now says 0 s.

Nothing else changes: a branch's overall wall-clock limit is still never handed to the analysis as its own deadline, and plain errors, a script's own timeouts and the curve / image runners behave exactly as on `main` (pinned by a guard test).

---

## Technical details

**#700, `swarm.py`.** At launch, `entry["_budget_s"] = fo.resolve_branch_budget({data_path, label}, budget, explicit=item_time_budget_s is not None)` for an analysis item with a `data_path`; every other item keeps `budget`. `_cancel_overdue_branches` already reads the per-entry `_budget_s`, so enforcement, the memory guard's rerun and reactions (launched through the same `launch`) follow. Tool schema text updated; one line in CLAUDE.md beside the fan-out rule.

**#718, `fanout.py`.** `_BRANCH_DEPTH_KEYS = ("profile", "targets", "time_budget_s")`:
- carried through `run_fanout`'s normalisation (`norm.append`), so they reach `_run_one_branch`'s `_depth`;
- recorded on the ledger entry as `depth` (as `_delegate` does);
- spread into the resume branch dict, so `resume_fanout` (plain and `retry_failed`) runs under them.

The fan-out tool schema already advertised `profile` and `targets` per branch. The pinned half of `test_a_fan_out_child_gets_no_run_deadline_from_the_branch_budget` now asserts the keys arrive and are on the entry; its first half (the branch budget is never the child's deadline) is unchanged.

**#719.**
- `hyperspectral_controllers.py`: `ctx.timeout_reached_s[hash(code_str)]` records the limit a script timed out at (sandbox timeouts only). A rerun of the same script starts there with `escalations=0`.
- `_locked_exec.escalate_timeouts`: `left()` returns the seconds as they are for the message and floored at 1 s for the limit.
- Tests: `_dynamic` stamps a deadline with `RunBudget(...).stamp(state)` and passes `qc_time_budget_s` to the controller constructor; the strict cases go through `HyperspectralAnalysisAgent.analyze(strict_replay=True)` (`_strict_frame`, zero model calls).
- New tests:
  - `test_a_ladder_retry_of_the_same_script_starts_where_it_timed_out`: escalation calls go from `[(1,None,2)]*3` to `[(1,None,2),(2,0,2),(2,0,2)]`;
  - `test_the_time_left_is_said_as_it_is`;
  - guard `test_what_this_change_leaves_alone`: a raising script and a script's own `TimeoutError` stay at `(1, None, 1)` on every attempt; `stage_and_run_adaptive` climbs `[1,2,4]` / `[1]` / `[1]`. It passes on `main` too.

Scope note: the same-script memory is in the hyperspectral controller only, where #715 put the escalation and #719 measured the cost; the curve and image runners are pinned unchanged by the guard.

**Verification.**
- Each new test fails with its fix reverted:
  - `test_a_datacube_series_item_gets_the_fan_out_branchs_budget`;
  - `test_a_retried_branch_keeps_its_own_depth`;
  - the updated #718 assertion;
  - the two #719 tests.
- Full suite against a merged-`main` worktree (7e6fa9ac), by failing-test ids: no branch-only failures. Four `test_litellm_completion_max_tokens` failures are on `main` too (LiteLLM cannot fetch its remote model registry in this environment).

, , .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e6fa9ac8d16`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.