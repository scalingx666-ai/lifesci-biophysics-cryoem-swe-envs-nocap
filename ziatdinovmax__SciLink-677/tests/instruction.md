# Issue

**Title:** Simulate: save the checkpoint on every per-turn call

## Description

## Summary

, following the fix suggested in the issue thread.

`SimulationOrchestratorAgent._auto_checkpoint()` held the `CHECKPOINT_INTERVAL` check internally, so every caller that saves per turn with a bare `_auto_checkpoint()` — `cli/shell/turn.py::save_checkpoint_quietly`, `server/runner.py`, and the `finally` in `run_task` — did nothing until a session reached 10 messages. Only the wall-clock-throttled per-tool save (`force=True`) wrote, so a short session restored a stale structure registry.

- `_auto_checkpoint()` now always writes; the `force` argument is removed (its only caller was the per-tool checkpoint).
- The interval check moves to the call site in `chat()`, matching the analysis and planning orchestrators. `chat()`'s own cadence is unchanged.

## Tests

`tests/test_per_tool_checkpoint.py`:
- `test_sim_interval_gate_still_holds_without_force` pinned the old behavior (a bare call inside the interval skips) and is replaced by:
- `test_plain_auto_checkpoint_saves_in_a_short_session` — parametrized over all four orchestrators. On `main` it fails for `simulate` only.
- `test_sim_short_session_restores_its_structures` — a 3-message session saved through `save_checkpoint_quietly` restores its `generated_structures`. Fails on `main`.
- `test_sim_chat_keeps_the_message_interval_cadence` — `chat()` still checkpoints only every `CHECKPOINT_INTERVAL` messages.

`tests/test_simulation_orchestrator.py::test_6_session_persistence` no longer sets `message_count = CHECKPOINT_INTERVAL` to get a write.

Also run: `test_run_task_cap_exhaustion.py`, `test_mcp_simulate_mode.py` (pass); `python tests/test_simulation_orchestrator.py` targeted tier is 13/14, where the one failure (`Orchestrator constructs`) is pre-existing on `main`: `EXPECTED_TOOLS` lacks `get_history_events` and `search_session_history`.

## Not in this PR

The separate gap noted in the issue thread — `chat()` called directly from Python on analysis/planning (and, after this change, simulate) has no end-of-turn save — is left as is.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01XYvkT6x9VFTBCyY33DMLPL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b988c7cdab5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.