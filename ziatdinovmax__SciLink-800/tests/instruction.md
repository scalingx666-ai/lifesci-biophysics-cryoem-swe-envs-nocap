# Issue

**Title:** Let Mission Control save documents as a named knowledge base

## Description

## Why

A session that indexed an uploaded report (e.g. a DOE assessment for a TEA) kept that index inside its own `planning/knowledge` folder. The next session started from nothing: the chat can attach a stored KB (`attach_knowledge_base`) or grow one (`add_to_knowledge_base`), but cannot create one, so reusing a report meant leaving the chat for `scilink kb create`. Nothing landed in `~/.scilink/knowledge_bases/`.

## What

- **`create_knowledge_base(name, paths, description="")`** meta tool → `MetaOrchestratorAgent.create_kb_from_documents` → `kb_store.create_kb`.
  - Embeds with the session's embedding model (its key and base URL) when one is set; otherwise builds a keyword-only (BM25) KB and says so in the result (`note`).
  - An existing name is refused (`FileExistsError` → JSON error).
  - The KB is **not attached**. The system prompt is refreshed, so it shows up in the detached-KB list with its sources; attaching stays an explicit `attach_knowledge_base(path=<name>)`.
  - The description restricts it to an explicit user request.
- **Shared servers refuse writes to the store.** `scilink.server.cli` sets `SCILINK_SHARED_STORE=1` under multi-user auth; `kb_store.store_writes_refused` makes both create and add raise `PermissionError` there, naming `scilink kb` for an operator.
- README: the Named Knowledge Bases paragraph covers creating from chat and the shared-server refusal.

## Testing

- `tests/test_meta_create_knowledge_base.py` (5): keyword-only build with no embedding model (and the detached-list entry in the prompt), embedding with the session's model/key/base_url, an existing name refused, create and add refused on a shared server, the tool's JSON for success and error.
- Full suite against a `main` worktree: same failures on both sides (pre-existing collection errors in `test_lammps_skill` / `test_literature_no_overwrite.py`, `test_mcp_meta_mode` order-dependent, live fan-out tests needing credentials); 275 vs 270 passed, the difference being the 5 new tests.
- Live in the web UI: asked Mission Control to save an uploaded PDF as a KB; it appeared in the store and was attached in a later session.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b3231dde398`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.