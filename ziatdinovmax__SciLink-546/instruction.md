# Issue

**Title:** Delegations tab: live mission-control view of the meta's ledger

## Description

## Summary

Mission-control sessions in the web UI now have a **Delegations** tab. It shows, live, what the meta agent has handed to which specialist and how those pieces of work relate.

- Delegations are grouped by specialist (analysis, planning, simulation, fusion) with the worker agents each one used, and each row shows its status, when it started, and how long it took or has been running.
- A row that took context from an earlier delegation shows "← #1" so you can follow how results flowed into later work. Fan-out branches, timed-out and resumed branches are tagged.
- Clicking a row reveals the task the meta sent, the specialist's summary and key findings, the files it produced (each opens in the Files tab), warnings, and any error.
- The tab updates while a turn runs: a delegation appears as soon as the meta opens it and flips to done when it closes, with the count in the tab label. It also survives a page refresh or a resume, since the ledger is restored from the checkpoint.

This is the last of the high-value Streamlit panels from the roadmap (the sidebar delegation tree plus the telemetry ledger) brought to the web UI. Verified live on a Bedrock Opus 4.8 meta session that delegated a two-spectrum peak-fitting analysis.

## Technical description

### Backend

- `scilink/server/delegations.py` (new). Read-only over the meta orchestrator's in-memory `_delegation_ledger`. `delegation_view(agent, session_dir)` returns `{"delegations": [row...], "sub_agents": {specialist: [worker names]}}`; each row carries index, mode, label, clipped task and summary, status, numeric `context_from`, `informed_by`, fan-out flags, fusion `labels`, timestamps, up to 12 findings, up to 40 files (session-relative when inside the session, so the tab can open them in the explorer), feature-table count, warnings, error. `ledger_signature(agent)` is the cheap change detector: a tuple of (index, status, completed_at, summary length, file count) per entry. Never raises; a malformed entry degrades to a minimal row. `sub_agents` reads only `agent_type` from `*_state.json` files that carry an `action_history`, the same source the Streamlit telemetry tab uses.
- `scilink/server/session_manager.py`: the snapshot carries `delegations` for meta sessions (null for other modes), so a fresh page or a resumed session renders the ledger immediately.
- `scilink/server/runner.py`: the turn watcher computes the ledger signature on its existing 2-second fs tick and emits a `delegations` SSE event with the full view only when it changes, plus one forced push after the turn thread finishes so the closing delegation of a turn is never missed.
- `scilink/server/app.py`: `GET /sessions/{id}/delegations` (same payload as the event; empty for non-meta sessions rather than an error) and `GET /sessions/{id}/telemetry` (the full `collect_session_telemetry` reader behind the Streamlit Telemetry tab, exposed for a future web Telemetry view).

### Frontend

- `api.ts`: `DelegationRow` / `DelegationView` types, `delegations` on the snapshot, `api.delegations(id)`.
- `useSessionEvents.ts`: `delegations` event.
- `App.tsx`: `delegations` in the session state (seeded from the snapshot, replaced by events), a third tab shown only in meta mode with a count badge, mounted alongside the other tab bodies so switching tabs keeps its expanded row.
- `DelegationsPanel.tsx` (new): grouping, status coloring (same palette as the Streamlit tree), context-flow edges, expandable detail, files opening through the existing `UIContext.openInFiles`, and a one-second ticker for elapsed time while any row is running.
- Styles appended to `app.css`.

The shipped bundle in `scilink/server/static/` is deliberately **not** rebuilt in this PR; #545 moves the bundle out of git and the release workflow builds it. From a checkout, `scripts/build_webui.sh` or `cd webui && npm run build` picks this up.

### Tests

Three new tests in `tests/test_web_server.py`: the view's shaping (relative file paths, clipping, non-numeric `context_from` dropped, fusion labels, sub-agent annotation, signature moves when a delegation closes, non-meta agent yields an empty view); the endpoints and snapshot field for meta and non-meta sessions plus the telemetry endpoint; and a fake meta agent that opens a delegation mid-turn and closes it just before returning, asserting the SSE ring holds a `running` event followed by a final `success` event. Suite: 41 pass. `tsc -b` clean.

### Live verification

Bedrock Opus 4.8, autonomous meta session, two spectra uploaded as a folder, one analysis delegation requested. The tab showed the row as running with a live elapsed counter within seconds of the meta opening it, and the finished state with summary, findings, and produced files once the specialist returned. Details in the PR comment below.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b74a17eca46d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.