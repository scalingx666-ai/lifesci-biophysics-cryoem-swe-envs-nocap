# Issue

**Title:** KB: record the embedding model that built a dense index; explain mismatches

## Description

## Summary

.

The "Dense KB retrieval failed ()" seen in the Bedrock/Titan web session was not an expired credential. The query embedding succeeded; what failed was the index search: the dense index had been built by a different embedding model (a different vector size), and FAISS rejects that with an error that has no message. Every query then paid for an embedding call, failed silently, and fell back to keyword search.

After this change:

- Every dense index records which embedding model built it. When a session with a different embedder attaches such a KB, the session says so once, names both models and the two ways out (start with the model that built it, or rebuild with yours), and uses keyword search from the start.
- An index written before this change gets a clear message on its first query ("query embeddings are 1024-d but the dense index is 3072-d …") instead of "()", and the KB stops embedding queries for an index they can never match.
- Every KB error line now carries the exception type and text, so nothing renders empty again.

Retrieval still never blocks generation: a KB is grounding, not a dependency.

## Technical description

### Evidence against the credential theory

- FAISS's Python wrapper guards `search` and `add` with a bare `assert d == self.d`, so a dimension mismatch raises `AssertionError('')`. A litellm/Bedrock auth failure carries a message. The empty `()` is the assert.
- `KnowledgeBase.retrieve` prints "❌ Error embedding query: …" before re-raising an embed failure. The issue's log shows the dense-path line, then the BM25 fallback, with no such line in between. The embedding call returned; the search asserted.
- Live on Bedrock: a Titan-built KB queried in a Titan session gives dense hits (14 checks in `scratchpad/live_631.py`, run against real Gemini, OpenAI and Bedrock Titan embeddings with the keys in the environment).

Which path produced the foreign index in that session cannot be confirmed from here (the session is not on this machine). Candidates: a named store KB created earlier with the store's default embedder (`create_kb` defaults to `gemini-embedding-001`, 3072-d), the meta's add-documents path embedding with the store manifest's model while the session queries with its own, or the plan-mode web sessions' shared `session_root/kb_storage` directory loaded by a later session with another embedder. The fix covers all of them at the KB level rather than per producer.

### Changes (`scilink/knowledge/knowledge_base.py`, `scilink/knowledge/rag_engine.py`)

- `save` writes `<prefix>.meta.json` next to the FAISS index: `embedding_model`, `dimension`, `vectors`. It shares the KB prefix, so the meta's `kb_cache` copy (`glob("default_kb_*")`) and the store's `_build_index_into` carry it along.
- `load` reads it into `index_built_with`. If the session's embedding model differs, `_dense_disabled_reason` is set, one print + `logging.warning` names both models and the remedies, and `retrieve` serves `retrieve_sparse` without an embedding call. A legacy index (no sidecar) loads as before.
- `retrieve` checks `query_embedding.shape[1] == index.d` before the search and raises `EmbeddingDimensionMismatch` with both dimensions and models, setting the same disabled flag so later queries skip straight to BM25. `build` applies the same check before appending to an existing index (a bare FAISS assert before).
- `describe_error(e)` renders `Type: message` (repr when the message is empty) at the build-embed, query-embed and `retrieve_context` fallback sites, with a DEBUG traceback.
- `build` records `index_built_with` and logs the index dimension and model.

### Deliberately unchanged

- A legacy index built by a different model of the same dimension still searches silently, as it did before; new builds carry the sidecar and are caught by name.
- No credential refresh: there is no evidence of an expiring token in this failure, and the diagnostics now name an auth error if one ever occurs.
- The web UI is untouched; the warning reaches the browser through the chat thread's log handler (and the print through the router).

### Tests

`tests/test_kb_embedding_provenance.py` (7 tests): sidecar contents and glob compatibility; same model keeps dense retrieval; model mismatch explained once and served by BM25 with zero embedding calls; legacy index names the dimension mismatch and stops re-embedding; appending with another model is a named error; `describe_error` never renders empty; a rebuild with the session's model re-enables dense retrieval. All 265 KB-related tests pass (`test_kb_*`, `test_knowledge_query`, and every test touching `KnowledgeBase` / `retrieve_context` / `rebind_kb`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e9d09915e1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.