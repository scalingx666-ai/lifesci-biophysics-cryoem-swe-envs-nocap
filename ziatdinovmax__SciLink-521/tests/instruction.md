# Issue

**Title:** Fan-out hardening: honest delegation status, cooperative branch cancellation, partial-donor harmonize

## Description

Four related reliability fixes for multi-dataset (fan-out) sessions, each live-validated on Bedrock.

**What changes for users:**

1. **Failed steps now say they failed.** Previously, if a delegated analysis died mid-turn, the session could record it as a "success" whose summary began with "❌ Error" — and later steps (status summaries, cross-dataset fusion) would treat it as a real result. Failures are now reported as failures, everywhere.

2. **A stuck parallel branch is actually stopped, not just written off.** When one branch of a parallel analysis exceeds its time budget, it used to be abandoned on paper while its thread kept running — burning API quota and holding memory that could block other branches. It is now cancelled: the thread winds down, its cancellation is recorded, and its memory is freed. The scientific verdict is unchanged (the branch is excluded from fusion exactly as before).

3. **Harmonized replay survives a partially-successful donor ().** In harmonized mode, the first dataset's approved analysis script is replayed on its siblings so results are method-comparable. If that donor run hit its step limit *after* approving a script, the whole group used to fall back to independent analyses. Now the approved script on disk is what counts: siblings still replay it, and the result is clearly labeled as coming from a partial donor. Donors are also nudged to complete their analysis in a single call, making this case rarer.

4. Internal cleanup: the identical plan-approval loop that existed in two copies now lives in one shared place (no behavior change).

---

**Technical details**

- **Honest status (`bb20cc65`).** Every orchestrator `chat()` has an emergency catch-all returning `"❌ Error: …"` as a normal reply; `run_task` reported that as success. `chat()` now sets `_last_chat_error` (mirroring the `_last_chat_hit_iter_cap` flag; analysis + planning — simulate's chat propagates exceptions and was already honest) and `run_task` flips status to `error`. Meta-side, `_close_delegation` derives status from the outcome: a `"success"` carrying an `error`, or a missing status, is recorded as `error`; explicit `"interrupted"`/`"cancelled"` pass through (pinned by `test_ui_stop`). Live: 5/5, including a control run against the installed release reproducing the old bug.

- **Cooperative cancellation (`33787de2`).** The budget path in `run_fanout` now fires a per-branch `threading.Event` alongside `kill_subprocesses_for_thread`. A thread-aware stdout/stderr wrapper (`_ThreadStopStream`, installed idempotently and deliberately never uninstalled so late stragglers still hit it) raises `AgentStoppedError` in the cancelled branch's thread on its next write; `_run_one_branch` converts it to a `cancelled` late result **only when it is that branch's own stop event** — a user-initiated stop stays unrecoverable and propagates. The branch's `finally` then releases its memory-admission slot. Cooperative, not preemptive: a branch inside one long LLM call notices when the call returns. Also fixes a latent unbound-`result` NameError in `_run_one_branch`'s `finally` on the propagating stop path. Live: 9/9 (both branches of a real fan-out cancelled under a 120 s budget; slots released).

- **Partial-donor harmonize, #519 (`7ee3a335`).** The donor-record search no longer requires the donor delegation's top-level `status == "success"`; the `task_success`-gated `dynamic_analysis_records.json` entries are the authority, and only a donor with no approved record triggers the loud independent-branch fallback. A partial donor is surfaced via `donor_partial`/`donor_status` in the `harmonized` result block, and harmonize-mode donor tasks carry a `PIPELINE-DONOR NOTE` (re-stamped into the ledger task). Interaction worth noting: the honest-status fix above makes error-reporting donors *more* common, so this gate change is its necessary complement. Live: 7/7 on sibling hyperspectral cubes (donor script found, follower replayed verbatim).

- **Gate extraction (`137f61ba`).** `base_controllers.run_plan_refinement_gate` replaces the byte-identical loops in the curve-fitting and image-analysis planning controllers; banner strings are parameterized so console output is unchanged.

**Tests:** 26 new offline tests (`test_run_task_error_status`, `test_fanout_branch_cancellation`, `test_plan_refinement_gate`, `test_harmonize_partial_donor`, plus new cancellation checks in `test_wallclock_budget_live`); all surrounding fan-out/meta/controller suites green. Two pre-existing issues observed and left alone: order-dependent pollution between `test_best_of_n_anchor` and `test_mcp_meta_mode` in large batches (identical on `main`), and `test_curve_fitting_orient_and_adapt` referencing the removed `_adapt_script_for_spectrum`.

No overlap with the #518 fit-mask work (hyperspectral-agent files only).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0f722b366d13`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.