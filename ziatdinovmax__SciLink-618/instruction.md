# Issue

**Title:** Default to no embedding model — keyword-only (BM25) grounding

## Description

## Summary

The default embedding model was `gemini-embedding-001`, so every plan and Mission Control session silently assumed a Google API key it usually never used. A Bedrock- or OpenAI-only session that then used a knowledge base *tried* the Gemini embed, failed, printed an alarming `❌ Error embedding` line, and fell back to keyword (BM25) search — functional but noisy, and a wrong-vendor default that is meaningless for a Claude/Bedrock session (Anthropic has no first-party embedder). The picker also showed `(default)` and `gemini-embedding-001` as two rows that did the same thing.

No embedding model is now the default, as a deliberate **keyword-only (BM25)** mode: a knowledge base is still built and searched, just by keyword over its stored chunks — no embedding provider or key needed, no failed call, no error. **Dense (semantic) retrieval is opt-in**: name an embedding model (a preset, a custom name, or a Bedrock/Titan/Cohere embedder). This matches the codebase's own rule that *retrieval is grounding, not a dependency*.

A store knowledge base built with a specific embedder is **not** silently re-embedded on the keyword-only default — that would attempt a possibly-uncredentialed embed and bring the noise back. The compatibility warning instead names the exact model and key to set for dense retrieval, keeping the choice with the user.

## Technical description

- `KnowledgeBase`: `embedding_model` defaults to `None`; a falsy model builds no embedding client, and `build()` makes an intentional keyword-only KB (chunks kept, `index=None`, clear message, no `embed_content` call). The query path's existing BM25 tier serves `retrieve()`.
- Session constructors (`PlanningAgent`, `PlanningOrchestratorAgent`, `MetaOrchestratorAgent`) and the CLI flags (`scilink plan` / `scilink explore --embedding-model`) default to `None`; the meta forwards `None` to its children.
- `embedding_compat_warning` renders cleanly for a keyword-only session — names the KB's build model and key for dense — instead of "embeds queries with 'None'".
- Sidebar: the default option is `(none — keyword-only)` with a caption; the `gemini-embedding-001` row is now a distinct choice, not a duplicate of the hidden default.
- `scilink kb create` (the explicit persistent-store build path, guarded by its own key check) keeps its dense default — untouched.
- Tests: `tests/test_no_model_embedding_default.py` (5) — keyword-only build with no client / no dense index / chunks kept / BM25 retrieval / no embed call; a named model still builds a dense client; the constructors default to `None`; the compat-warning text. Full suite: no new failures vs main.

### Live checks (Bedrock, real planning orchestrator)

- **No model, only the Bedrock key in the process**: built and queried a knowledge folder keyword-only — `embedding_client` `None`, no dense index, chunks kept, the correct chunk returned via BM25, and neither `❌ Error embedding` nor `Generating embeddings` in the logs. The previous noisy failed-Gemini path is gone.
- **The same session with `text-embedding-3-small` (OpenAI key)**: a dense index was built (`LiteLLMEmbeddingModel`, `Generating embeddings` logged) and it answered.
- The meta forwards no-model to its planning child (`kb_docs.embedding_client is None`).
- Web UI: the default reads `(none — keyword-only)` with the caption; `gemini-embedding-001` is a separate row.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79237c5e7748`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.