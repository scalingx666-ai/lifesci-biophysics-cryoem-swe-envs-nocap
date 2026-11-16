# Issue

**Title:** Planning: a session's copy of a store KB follows the store

## Description

> Stacked on #683, which it builds on (`snapshot_kb`). Review after #683; this PR's own change is the last commit.

## Summary

When a SciLink session uses a named knowledge base, it works from its own copy. If documents were added to that knowledge base while the session was open, whether by `scilink kb add` in another terminal or by another session, the session kept the old copy. Its retrieval then never saw the new documents. Now each planning turn first checks whether the knowledge base has changed since the session copied it, and takes a fresh copy if so.

## Technical description

**The gap.** The planning orchestrator seeds `<session>/planning/kb_cache` from a store KB once, at construction (only when the cache is empty). After that it binds the planner to the cache and never looks at the store again. The #683 review saw it live: after a `kb add` from another process, the planning specialist answered that the KB "does not contain" the new document.

**The change.**
- **`kb_store.snapshot_kb`** writes `.snapshot_of.json` into the copy, holding a SHA-1 of the manifest of the publish it accepted. The manifest changes with every publish; `updated_at` has only one-second resolution.
- **`kb_store.snapshot_is_current(kb_dir, dest)`** compares that digest with the store's current manifest. A copy with no marker counts as current, so an older cache is not re-copied on every turn.
- **`PlanningOrchestratorAgent._refresh_store_kb()`** runs at the start of every `chat()`, which `run_task` goes through too. When the store has moved on, it snapshots again, updates `_kb_store_manifest`, calls `planner.rebind_kb(...)` and prints one line. When nothing changed it costs a manifest read. If the re-copy fails, it keeps the session's copy and logs a warning.
- **Session-embedded documents:** anything the session embedded into its own copy (`_ensure_kb_is_ready` → `_build_and_save_kb`) is dropped by a refresh. It is re-embedded on next use, because the planner embeds whatever its KB lacks, so the cost is time and nothing is lost.

**Tests (`tests/test_kb_store_concurrency.py`).**
- A snapshot knows when the store has moved on, and a missing marker counts as current.
- A planning session refreshes once per publish, rebinds the planner, and sees the new source.
- `chat()` checks the store before calling the model.

**Live verification** (Bedrock: Titan v2 embeddings, `us.anthropic.claude-opus-4-8`). Both runs used a KB with one document; a second document was added by another process.

| check | with this PR | without |
|---|---|---|
| the planner's own retrieval, `planner.kb_docs.retrieve("How are the batch A7 vials labelled for storage?")` | finds the added document (the copy holds 2 chunks) | misses it (the copy holds 1 chunk) |
| a meta session: turn 1, then the external add, then turn 2 asking for the added fact | the refresh fires ("updated since this session copied it; refreshed (2 sources)"); planning names the fact | planning also names the fact |

The meta-session row came out the same both ways, and the reason is worth stating. With #686 in place, the planning agent is told its knowledge directory, and in this run it read the store's files directly with `read_file`. The session copy matters where the planner retrieves from its own KB, as plan generation does, and the first row checks exactly that path.

- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `152260baf834`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.