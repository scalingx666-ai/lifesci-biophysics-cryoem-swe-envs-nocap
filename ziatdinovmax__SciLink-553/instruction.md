# Issue

**Title:** MCP tab: connect servers to the session's agent

## Description

## Summary

The web UI has an **MCP** tab in every mode. Connect an MCP server — a local command, an SSE endpoint, or a streamable-HTTP endpoint with optional headers — and the agent can use its tools in chat right away. Each connected server shows as a card listing the tools it registered, with a disconnect button.

Two things are deliberately absent. No tool-file uploader, per the decision recorded in the roadmap: you hand the agents code either as a script attached in chat (which the agent adapts) or as an MCP server (run exactly as written, in any language). And no list of the agent's own built-in tools: in a mission-control session that would be only the meta's routing tools, not what the specialists can do, so it gave a partial picture that read as the whole.

Verified live: a small MCP server exposing a calculator and a fake cryostat readout was connected to a mission-control session; the agent used both in one turn and reported the readings only those tools could produce.

## Technical description

### Backend

- `scilink/server/tools_api.py` (new): `tool_inventory(agent)` reads `_external_tools` and `_mcp_connections` (name, transport inferred from `command` vs `transport`, tool names from each connection's `tool_schemas`), plus `mcp_supported`. `connect_mcp` validates name / transport / command-or-url, returns 409 for a duplicate name, wraps the agent's `connect_mcp_server` (stdio command split into argv; url with transport and headers), and maps failures to 400. `disconnect_mcp` wraps `disconnect_mcp_server` with 404 for an unknown name. `${VAR}` expansion in header values (Streamlit parity, keeps tokens out of the form) is gated on `local_files`, so a remote user cannot read the server's environment through it.
- `scilink/server/app.py`: `GET /sessions/{id}/tools`, `POST /sessions/{id}/mcp`, `DELETE /sessions/{id}/mcp/{name}`; connect and disconnect return the refreshed inventory. `scilink/server/schemas.py`: `MCPConnectRequest`.
- All three orchestrators expose the same MCP methods, so the tab works in analyze, plan, and meta sessions; an agent without them reports `mcp_supported: false` and the form is replaced by a notice.

### Frontend

- `ToolsPanel.tsx` (new): transport radio (stdio / sse / http) with a hint, name + command-or-URL row, headers field for the URL transports, connected-server cards with tool chips and a disconnect button, and a list of any other externally registered tools. Fetches on tab activation and refreshes from the connect / disconnect responses.
- `App.tsx`: `MCP` tab for every mode, mounted alongside the others. `api.ts`: `ToolInventory`, `tools`, `connectMcp`, `disconnectMcp`. Styles appended.

### Tests

Two new tests in `tests/test_web_server.py`: the full lifecycle against a fake agent (inventory shape, stdio argv split, http transport with headers, 409 duplicate, 400 on connection failure / empty name / unknown transport / unsupported agent, disconnect and 404, no `builtin` field) and env-expansion gating. Suite: 44 pass. `tsc -b` clean.

### Live verification

FastMCP stdio server (`add`, `cryostat_temperature`) connected to a Bedrock Opus 4.8 meta session through the endpoint: 2 tools registered. One chat turn asked for both stage temperatures and a sum; the verbose log shows three tool calls and the answer was "The sample stage reads 4.217 K, the shield stage reads 41.9 K, and 19.5 + 22.75 = 42.25", the fake readout values. In Chrome the tab showed the server card with both tools, and disconnect removed the server.

### Docs

`react_web_ui.md`: MCP tab bullet, the three endpoints, not-yet-ported list; roadmap item marked done.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8053feee305f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.