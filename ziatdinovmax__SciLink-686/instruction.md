# Issue

**Title:** Planning keeps its workspace paths when the autonomy level changes

## Description

## Summary

A planning agent is told in its instructions where its data, knowledge and code folders are. When its autonomy level changed, which happens at the start of every delegated task, that part of the instructions was dropped. So a planning run started by the meta agent, or through `run_task`, did not know its knowledge base existed. Asked to use facts recorded there, it said no knowledge base was accessible. Now the folders stay in the instructions when the level changes.

## Technical description

**The defect.**
- `PlanningOrchestratorAgent.set_autonomy_level` rebuilt the system prompt from `get_system_prompt(level, external_tools)` alone. The constructor and `_rebuild_system_prompt` both append `_build_workspace_context()`, the objective and data, knowledge and code directories for AUTOPILOT and AUTONOMOUS.
- `run_task` calls `set_autonomy_level` on every call, and the meta builds its planning child in CO_PILOT (whose workspace block is empty by design). So a delegated planning run always ran with the block missing.
- Both rebuild paths were last touched in d1c2f2ec5, and nothing explains the omission.

**The change.** `set_autonomy_level` now calls `_rebuild_system_prompt()` after setting the level. CO_PILOT still gets no workspace block, as designed. Human-feedback gating is unchanged.

**Tests: `tests/test_planning_prompt_workspace.py`.**
- Switching from CO_PILOT to AUTOPILOT or AUTONOMOUS names all three directories.
- AUTOPILOT to AUTONOMOUS keeps them.
- Switching to CO_PILOT gives the plain CO_PILOT prompt.
- Human feedback still follows the level.

The three workspace tests fail on `main`. `tests/test_mcp_hitl.py` (autonomy propagation) passes unchanged.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`, KB built with `amazon.titan-embed-text-v2:0`):
- **Setup.** A planning agent was built in CO_PILOT with `knowledge_dir` set to a named KB, as the meta builds its child. It then ran an AUTONOMOUS `run_task`: design one XRD re-check "using the lab's own instrument and storage procedure as recorded in the attached knowledge base". Two facts exist only in the KB: an asset tag and a vial code. Only the agent's own answer and plan files were searched, not the KB's cached copy.
- **`main`:** two tool calls (`list_workspace_files`, `query_knowledge_data`), then gave up in 17 s. Neither fact.
- **This branch:** read the knowledge directory it was now told about, called `generate_initial_plan`, and named both facts in 76 s.
- Full offline suite: no failures that `main` does not have.

**Found, not changed.** Through the meta the same request still did not reach planning. `MetaOrchestratorAgent._kb_note` lists only knowledge bases that are *not* attached and returns `""` when one is. So the meta's model is never told that a KB is attached, although it passes `knowledge_dir` to its planning child. It searched uploads and session state instead of delegating. Noted for a follow-up; it is a separate change to the meta's prompt.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19b9140861bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.