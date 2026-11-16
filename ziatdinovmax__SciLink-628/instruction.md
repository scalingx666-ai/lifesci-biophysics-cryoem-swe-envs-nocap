# Issue

**Title:** Web app: route background-thread output (and Stop) to the session stream

## Description

## Summary

.

In the React web app, progress printed by background threads never reached the browser. The visible case: a `search_literature` call showed its setup lines and then went silent for 10-15 minutes, because the periodic "⏳ N still running" ticks came from a helper thread the server only ever showed on its own console. The same gap hid the narration of parallel fan-out branches in a meta session, and it also meant the Stop button did not reach those branch threads: stopping a fan-out stopped the coordinator while the branches kept computing.

After this change:

- Literature searches show their progress ticks and the per-search "Submitting … query" lines in the browser.
- Parallel fan-out branches narrate into the session's log stream, including best-of-N candidate lines inside a branch.
- Stop now reaches every thread working for the session, including a branch that was in the middle of a model call and a branch still queued for memory headroom. Stopped branches stay "running" in the ledger, which is what `resume_fanout` treats as interrupted and resumable.
- A separate one-line fix: single analysis delegations from a meta session (`delegate_to_analysis`) were failing since today's embedding-endpoint commit, because the meta passed a keyword the analysis child does not accept.

CLI sessions are unchanged: no new log prefixes, no trimming, only routing and stop propagation.

## Technical description

### The contract

`scilink/server/stdout_router.py` routes each `write()` by the calling thread's *effective* thread, and the web turn's log handler keeps a record only when its thread resolves to the chat thread. So only a thread registered through `log_context` reaches the browser. Streamlit's `OutputCapture` swapped the process-global stream and saw every thread; the per-thread router (built for concurrent-session isolation) sees only attributed ones. This PR makes attribution the explicit contract and gives tools two helpers so it is hard to get wrong.

### `scilink/utils/log_context.py`

- `start_attributed_thread(target, ...)`: drop-in for `threading.Thread(...).start()`; the thread is a worker of the caller's session for the life of its target, and a session stop (`AgentStoppedError` from a routed write) ends it quietly instead of an "Exception in thread" traceback on the console.
- `attributed_to_current(fn, ...)`: wraps a callable for `pool.submit`; registration is scoped to each call, so a reused pool thread is unattributed again afterwards and no stale mapping survives thread-id reuse. Exceptions propagate unchanged.
- `effective_thread` now follows a worker chain (bounded), so a best-of-N candidate inside a fan-out branch resolves to the chat thread. One-level registrations behave exactly as before.
- Persistent stop: `stop_workers_of(chat_tid)` / `clear_stop` / `worker_stopped`. The stop signal used to be the route itself and the turn's log handler, both removed when the chat thread exits. Live, the coordinator ended 12 s after Stop while a branch inside a model call kept fitting for minutes, and a second branch admitted after the Stop started narrating. Now a stopped chat thread stays flagged until its last attributed worker unregisters; a worker of a stopped turn raises on its next routed write (route or no route) and on its next log record, via the always-installed record factory, which is the one hook every worker's records pass through in the emitting thread. A fresh turn on a reused thread id clears the flag on entry.

### Call sites

- `orchestrator_tools.py` (`search_literature`): the heartbeat thread starts through `start_attributed_thread`; each pooled search is submitted through `attributed_to_current` (its "Submitting … query" and result lines are logged from the worker).
- `meta_agent/fanout.py`: both `pool.submit(_run_one_branch, ...)` sites wrap the branch with `_attributed_branch` (routing only: `prefix=False`, no `[tag]` log prefix, no panel trimming). `_run_one_branch` itself is unchanged.
- `server/stdout_router.py`: `RoutedCapture.request_stop` flags the turn's workers; `__enter__` clears a stale flag; `_RoutingStream.write` raises for a stopped worker when its route is gone. Module docstring states the contract.
- `ui/output_capture.py`: the Streamlit capture gets the same two calls (its `TeeStream` is restored at turn exit, same gap).
- `meta_agent/meta_orchestrator.py`: drop `embedding_base_url=` from the analysis-child constructor (30940a44 forwarded it to both children; only the planning child takes it, and the analysis orchestrator never uses embeddings). The fan-out child factory never passed it.

### Audit of other thread spawns

`mcp_client` event loop, `memory_api` jobs, the runner's own watcher, the Streamlit chat thread and `_cif_index` parsing are server-side or non-narrating and were left alone. The best-of-N candidate pools in the curve and image controllers already register.

### Tests (offline)

- `tests/test_log_context_attribution.py` (new): bare thread is dropped vs attributed thread routes; unregister after target; quiet exit on session stop; pool-scoped registration; exception propagation; stop reaches a pool worker; worker chain resolves to the chat thread; bounded chain; stop persists after the route is gone on both print and log paths; a worker attributed after the Stop is stopped; unrelated threads and a fresh turn untouched; the Streamlit capture gets the same guarantee.
- `tests/test_fanout_stdout_routing.py` (new): branch prints reach the coordinator's capture (and the console); Stop reaches the branches in two shapes: chatty (raises while the route exists) and late (silent past the coordinator's exit, the live shape).
- `tests/test_literature_cross_domain.py`: heartbeat ticks and the workers' "Submitting" records reach a `RoutedCapture`; the stub gains `search_for_technique_limitations` (the whole file failed on main since that search type landed: 16 tests) and its wedged-search stubs are released in `finally` so no attributed straggler outlives a test; a source-text assertion updated to the new submit line.
- `tests/test_meta_analysis_child_construction.py` (new): the meta builds a real analysis child with `embedding_base_url` set.
- Full suite, three runs on the branch vs one on main (live tests excluded, plus four files that do not collect in this environment): every failure on the branch also fails on main; the branch fixes the 16 literature tests. The remaining main failures are environment-specific (HPC, Materials Project, docx) or the pre-existing import-time `logging.disable(CRITICAL)` pollution from three test modules.
- Offline stress: 160 concurrent sessions × (8 helper threads + 6 pool workers + 6 nested workers) × 300 lines, 960k lines: every session captured exactly its own lines, console got all of them, no attribution or route left behind, no thread tracebacks.

### Live verification (Bedrock, real web turn runner via `SessionManager.create` + `runner.start_turn`, SSE events collected)

- A, plan mode, real FutureHouse search (Sonnet 4.5 on Bedrock, Opus 4.8 was returning 503 for ~15 min): three heartbeat ticks arrived on the SSE log stream and in the final message's verbose log; nothing left attributed.
- B, meta fan-out over a two-temperature Raman series (Sonnet 4.5): branch child narration (5 child tool-call lines) reached the stream; both branches succeeded. Its Stop turn documented the gap: coordinator ended in 12 s, the running branch kept going 193 s later with two workers still attributed. That is what the persistent stop fixes.
- C, two sessions in one process at once (Opus 4.8): a plan session with a real search next to a meta fan-out with best-of-2 candidates per branch. Four ticks on the plan stream, no fan-out lines leaked into it; branch narration plus five `[cand_00]` candidate lines on the meta stream, no literature lines leaked into it; both branches succeeded; nothing left attributed.
- D, fan-out Stop with the fix (Opus 4.8): Stop with one branch running and one queued; coordinator ended in 8 s, both branch slots released in 8 s, no attribution, stop flag cleared, no subprocess, no thread traceback.
- E, Stop during a real search (Opus 4.8): the workers' "Submitting" records reached the stream; after Stop the heartbeat thread ended quietly on its next tick (161 s), no traceback, the session shows "Planning stopped by user". The chat thread itself is parked in the search's bounded wait and only ended when the remote search returned (12 min): a pre-existing limit of the print-driven stop, not changed here. One search worker was still polling the remote service when the scenario finished and remained flagged, which is the intended state until its next log record ends it (covered offline).

### Known edge, deliberately not addressed

A worker abandoned by a timed-out search stays attributed to its chat thread id until the remote call returns. If that id is reused by a later turn, the straggler's final log line would land in the new turn's log. This was already true of any abandoned worker and only affects lines that were previously dropped entirely.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37759f7e2698`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.