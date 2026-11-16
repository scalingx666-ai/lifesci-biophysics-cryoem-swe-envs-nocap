# Issue

**Title:** Per-member refinement context + ClusterExecutor submit/poll + cancel-aware wait (#745 Step A)

## Description

## What

Step A of #745, **with dispatch still serial** — the conflict-free groundwork that also fixes the orphaned-job-on-Stop bug today (per your review on #745).

- **Per-member refinement context.** A `parallel=True` fan-out stage now refines each member in its own `RefinementContext` (fresh `cycle` + `history`) via `_member_context`, merging the per-member histories back into the shared context in member order after the stage settles. With serial dispatch this reproduces the shared-context behavior **exactly** (guard tests: a one-worker fan-out and a `parallel=False` stage give the same history and member record) — and it removes the shared-`ctx.cycle` / shared-`_stalled` entanglement that would break once members run concurrently.
- **`ClusterExecutor` submit/poll seam.** `run()` splits into `submit(...) -> handle` and `poll(handle)`, with `run()` keeping today's submit-then-wait contract on top — the non-blocking seam #696's job handle will use.
- **Cancel-aware wait.** The `run()` poll loop takes an optional `cancel_check` and cancels the scheduler job when it fires, and a `KeyboardInterrupt` during the wait cancels the job before re-raising — so a Stop no longer leaves N orphaned allocations on the cluster.

## Tests

Per-member-context guard tests (serial reproduces main; member-order merge); `ClusterExecutor` submit/poll, cancel-mid-wait, and submit-failure handle; existing refinement + executor suites unchanged.

Addresses #745 (Step A). Concurrent dispatch (Step B) and the `run_many` / #696 async instance are the follow-ups outlined there.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47a3de5f8a5c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.