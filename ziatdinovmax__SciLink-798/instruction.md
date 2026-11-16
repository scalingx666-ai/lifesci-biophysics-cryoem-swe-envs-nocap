# Issue

**Title:** Show each MCP tool's description and parameters in the MCP tab

## Description

## Summary

In the MCP tab, a connected server's tools were shown as chips whose only description was a hover tooltip, flattened to one line and cut at 600 characters. Clicking a chip did nothing, so there was no way to read what a tool does or what it takes.

Each tool chip is now a button. Clicking it opens a panel under the chips with:

- the tool's description as the server wrote it, line breaks kept (capped at 8000 characters);
- its parameters from the input schema: name, type, whether it is required, and the parameter's own description.

One tool is open at a time; clicking the open chip again closes it.

## Changes

- **`server/tools_api.py`**: `tool_inventory` adds `tool_details` to each MCP server entry, beside the existing `tools` name list, so nothing that reads `tools` changes. Parameter types given as a list or through `anyOf` are joined (`number | null`).
- **`webui/src/components/ToolsPanel.tsx`**: chips become toggle buttons, plus a `ToolDetailCard` for the open tool. A server entry without `tool_details` falls back to the one-line description.
- **`webui/src/api.ts`**, **`webui/src/app.css`**: the `ToolDetail` type and the styles for the chip and card.

## Testing

- `tests/test_web_server.py::test_tools_inventory_and_mcp_lifecycle` now checks `tool_details` for a tool with a multi-line description, a required parameter and an `anyOf` parameter. The full `test_web_server.py` passes (54), and `tsc -b` is clean.
- Read the inventory from a real stdio MCP server (the bundled demo instrument server): `acquire`'s three parameters come through with their types and descriptions.
- Clicked through the tab in the running web UI with a connected server.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5424e29c1f63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.