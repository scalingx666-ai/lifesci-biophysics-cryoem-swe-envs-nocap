# Issue

**Title:** Checkpoints survive a failed write; delegations never share an index

## Description

## Summary

Each analysis, planning and simulation session saves a checkpoint and its chat history as it goes, so it can be resumed. If a save failed half way (the process was killed, or one value in the state could not be written), the file was left truncated. That destroyed the last good checkpoint as well, and resuming then found nothing it could read. Now a save either replaces the file completely or leaves the previous one untouched.

Separately, delegation numbers in the meta agent's list are now allocated under a lock. On `main` this is not yet a live race (the meta runs tool calls one after another, and fan-out already allocated under the lock); it is groundwork for running several delegations at once (#676).

Stage 0 of the agent-swarms proposal (#676).

## Technical description

**Checkpoints and histories.**
- `scilink/utils/text_io.py` gains `atomic_write_json(path, obj, **dumps_kwargs)`: `json.dumps` first, then the existing `atomic_write_text` (a same-directory temp file and `os.replace`).
- A serialization error therefore raises before anything on disk is touched. `json.dump` into `open(path, "w")` truncated the file first.
- Every writer of these files now uses it: `_auto_checkpoint` and `_save_history` in `AnalysisOrchestratorAgent`, `PlanningOrchestratorAgent` and `SimulationOrchestratorAgent`; the analysis and planning `save_checkpoint` tools, which now call `_auto_checkpoint` (one schema); and the meta's checkpoint and chat history. `atomic_write_text` creates its temp file with `O_EXCL` at `0o666`, so it never reads the process-wide umask.

**Ledger index.**
- `MetaOrchestratorAgent._open_delegation` read `len(self._delegation_ledger) + 1`. It ran `_infer_context_sources` (non-trivial) and appended afterwards, and only fan-out preallocation held `_fanout_lock` around it.
- It now takes the lock itself. The body moved unchanged to `_open_delegation_locked`.
- `_fanout_lock` becomes an `RLock`, because fan-out preallocation opens its branch entries while holding it. Its comment now says what it serializes.

**Tests: `tests/test_atomic_child_checkpoints.py`.**
- `atomic_write_json` keeps the previous file whole on failure and leaves no temp file.
- For each of the three orchestrators, parametrized: a failed checkpoint keeps the last good one, and a failed history save keeps the last good one. The failure is a circular reference, which fails even with `default=str`.
- 12 threads opening delegations, with a slow provenance step, get distinct indices 1–12.
- Fan-out-style re-entrant preallocation does not deadlock.
- The real `__init__` builds an `RLock`.

The 7 behaviour tests fail on `main`. `tests/test_delegation_provenance.py`'s orchestrator shell gains the lock attribute.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`, one autonomous meta turn, 940 s):
- The meta fanned out over a Raman spectrum and an XRD pattern of one sample (two concurrent analysis branches, so fan-out preallocation took the now-reentrant lock and opened its entries through the locked `_open_delegation`). It fused them, then delegated to planning and to simulation.
- Ledger: indices 1–5 in order (two branches, fusion, planning, simulation), all `success`.
- All 10 `checkpoint.json` / `chat_history.json` files under the session parse; no `.tmp` leftovers.
- Each restored from its checkpoint: the meta (same ledger), both analysis branches (same result counts), planning, and simulation (1 structure).
- Full offline suite: no failures that `main` does not have. Four timing-sensitive `test_per_tool_checkpoint` cases that failed in the `main` run passed here; they vary from run to run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `90dac93ce597`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.