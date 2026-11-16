# Issue

**Title:** Programmatic session memory: events.jsonl + bounded history search

## Description

## Summary

Long chat sessions lose their own middle: once the conversation grows past the context-management limit, older turns are trimmed away and the agent cannot get them back — a fit result from sixty turns ago, a parameter the user pinned early on, or what a specialist actually did three delegations back are simply gone, and the agent either asks the user to repeat themselves or redoes the work. This PR gives every session a durable, searchable memory of its own actions: each tool call is recorded as one compact line in a per-session `events.jsonl`, and two new tools let the agent grep that record and drill into specifics — so recalling something old costs context proportional to the question, never to the session length. When trimming happens, the trim notice itself now tells the model the details are recoverable by search.

Implements #462 (phases 0–3 of `session_event_log_plan.md`, included in the PR). The pattern (append-only structured action log, queried programmatically, never read whole) follows the PRO-LONG result (arXiv:2607.20064) — adopted as a pattern, not a harness.

Live highlight (Bedrock opus-4-8): with a fit result genuinely evicted from context, the agent — unprompted, routed only by the trim marker and tool description — searched its history, cited "event n=1", and answered with the exact center and R² instead of re-running the analysis or asking the user. In a fan-out session, the meta recovered the exact generated script path and R² of one branch from the *child's* log via scoped search, information the delegation ledger's summaries don't carry.

---

**Technical details**

*Phase 0 — writer.* New `scilink/session_events.py`; thread-local binding mirrors `hitl.py`'s feedback log exactly: `chat()` binds `<base_dir>/events.jsonl`, `run_task` saves/restores the caller's binding (a meta delegation logs child events to the child's session), unbound threads no-op, and the writer never raises into the tool path. Wired at all four orchestrators' `execute_tool` chokepoints via a thin wrapper over the renamed `_dispatch_tool`. Every field is bounded at write time (args ≤200 chars with `api_key`/`token`/`secret` kwargs redacted; summary ≤300; ≤8 files with tail-preserving path clips so filenames stay searchable); `n` is a monotonic per-log counter that resumes across restores.

*Phase 1 — access tools.* `search_session_history(pattern, max_hits)` (capped grep; most-recent hits when over cap; excludes its own past invocations) and `get_history_events(start_n, end_n)` (≤40 events / ~8 KB drill-down) on all four orchestrators, registered through one shared registrar (the `utils/file_edit.py` shape — shared engine, thin per-mode registration).

*Phase 2 — trim pointer.* The trim summary marker on analysis, planning, and meta gains one sentence routing the model to the search tool; the simulation orchestrator's previously *silent* slice-trim gains the same marker.

*Phase 3 — meta reach-through + fan-out branches.* The meta's search gains `scope: own | children | all` — children globs `{analysis,planning,simulation,fanout}/**/events.jsonl` (persistent children, per-delegation dirs, fan-out branches), labels each hit with its session, and applies global caps across the union; `get_history_events(session=...)` follows a labeled hit into that child's log. Fan-out worker threads bind to the meta's log and record a branch-tagged `fanout_branch` lifecycle event, while each branch child's tool calls stay in its own session log.

*Non-goals honored:* no RAG/embedding layer; no changes to `chat_history.json`, checkpoints, or the delegation ledger; Phase 4 (measuring whether the generic search already prevents a long BO campaign from re-suggesting failed conditions) is intentionally left as the plan's measure-first follow-up.

**Verification**

- Offline: 25 new tests (`tests/test_session_events.py`, `tests/test_history_search_tools.py`) — bounded fields, redaction, unbound no-op, thread-locality, binding restore, counter resume, writer-exception isolation, caps, honest nulls, malformed-line tolerance, registration on all four, trim markers, children-scope labeling and drill-down, branch-tagged events via the offline fan-out harness. Regression sweeps green: fan-out robustness 62/62, meta dispatch guards 13/13, UI-stop/MCP/edit-file/workspace/planning 38, sim delegation 3.
- Byte-identical guarantee: system prompts hash-identical to main on all four orchestrators; tool schemas are main's plus exactly the two appended declarations (the meta's additionally declares `scope`/`session`).
- Live (Bedrock opus-4-8), one scenario per phase: (0) meta + real delegation → both logs written with correct binding isolation, monotonic indices, bounded valid-JSON lines; (1+2) the trimmed-fact recall scenario above, 5/5; (3) real 2-branch fan-out then reach-through, 6/6 — four scoped searches, child-log drill-down, no re-runs.

Field note for reviewers: the trim *trigger* in the chat loops is a hardcoded `len(messages) > 120`, independent of `MAX_HISTORY_MESSAGES` (recorded in the plan doc; a possible follow-up cleanup).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bf34534b9ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.