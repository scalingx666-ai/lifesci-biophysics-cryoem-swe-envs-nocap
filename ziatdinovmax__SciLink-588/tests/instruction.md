# Issue

**Title:** Web UI polish: session-relative paths, readable graph, tab order, memory folds, log stand-down

## Description

## Summary

A batch of small usability fixes across the web UI:

- **Paths read session-relative everywhere.** Telemetry arguments and results, worker actions, analysis reports, and the delegation tasks, summaries and findings no longer print the absolute session root; the memory panel shows its store as `~/.scilink`.
- **The delegation graph stays legible.** Past 1100 px it keeps its natural size and scrolls, with a "Fit to width" / "Actual size" toggle.
- **Sidebar credential hint** puts the environment variable in monospace on its own line.
- **Tab order** is Chat, Files, Telemetry, Skills, MCP.
- **Memory panel**: the script-bank and review-inbox stages start collapsed when nothing waits for review; the pipeline strip is weighted; success notices fade on their own.
- **The verbose log no longer lingers under a fresh answer.** The last log flush arrived a moment after the reply and showed beneath it until the idle status followed; the client now stands the log down on the answer itself.

## Technical description

- `telemetry.py`: `relativize(obj, root)` walks dicts/lists/strings; applied to `agents`, `analysis_reports` and `tool_sequence`. `delegations.py`: `_rel_text` on task/summary/findings/warnings, applied before `_clip` so a clipped path keeps a whole prefix. `memory_api.py`: `_tilde` for `home`.
- `DelegationGraph.tsx`: `fit` state (default: width ≤ 1100), `.deleg-graph.fit` carries the max-width scaling, a `.deleg-graph-fit` link toggles it.
- `Sidebar.tsx` / CSS: `.cred-hint code` as a block. `App.tsx`: Telemetry button moved before Skills.
- `MemoryPanel.tsx`: `<details class="mem-stage-fold" open={inbox_total > 0}>` around stages 1 and 2 (stage headings become summaries, the old h4s become lead lines); a `useEffect` fades `ok` notices after 6 s. CSS: `.mem-pipeline` 14 px / 600.
- `App.tsx` reducer: new `answered` flag — set with `liveLog: ""` on `assistant_message`, cleared on `user_message` and on `status: running`; `log` chunks are ignored while answered.
- Tests: `relativize` (telemetry reader), session-relative task/findings (delegation view), tilde home (memory). Web suites: 53 pass.

### Verified in Chrome

Tab order and the two-line credential hint; the 45-delegation graph at natural size with the toggle; the memory folds and weighted strip; telemetry args such as `uploads/spectrum.npy` and a delegation task naming `uploads/spectrum.json` in place of absolute paths; and, on a real tool-using turn, a DOM probe recorded the live log pane present at 0.6 s and gone in the same 100 ms sample in which the assistant message appeared at 11.0 s.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `68b3d931739e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.