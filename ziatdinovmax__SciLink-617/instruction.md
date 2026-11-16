# Issue

**Title:** Embedding base URL: an endpoint for the embeddings only

## Description

## Summary

The base URL was a session-wide switch: once set, the chat model and the embeddings both went through that proxy with the main key, so "chat direct, embeddings through a proxy" (or two different proxies) could not be configured. A new optional **embedding base URL** names an OpenAI-compatible endpoint for the embeddings alone, authenticated with the embedding API key (the main key when that is blank). The chat model keeps its own route. It is available in the React sidebar (plan and Mission Control), as `--embedding-base-url` on `scilink plan` and `scilink explore`, and in the session API. Without it, behaviour is unchanged.

## Technical description

- `PlanningAgent`: `embedding_base_url` selects the knowledge-base clients' endpoint (`OpenAIAsEmbeddingModel` at that URL with the embedding key, falling back to the main key with a warning); otherwise the previous rules (main proxy with its key, the embedding key ignored with a warning; or the vendor via LiteLLM).
- `PlanningOrchestratorAgent` / `MetaOrchestratorAgent`: new parameter, stored and forwarded (the meta to both planning-child constructions); the "embedding key ignored" rule applies only without an embedding base URL; the meta's `add_documents_to_kb` embeds through `embedding_base_url or base_url`.
- Web: `CreateSessionRequest.embedding_base_url`, `SessionManager.create/resume`, both initializers, and `/api/v1/config` returning `embedding_credential.endpoint` (`vendor` | `base_url` | `embedding_base_url`, with `proxied` kept). Sidebar: the field, the hint per endpoint, start and resume payloads. CLI flags on plan and explore. Docs: web UI guide and CLI reference.
- Tests: `tests/test_embedding_base_url.py` (5) for the routing and key rules on the agent and the forwarding on both orchestrators; the config endpoint states and the request wire in `tests/test_web_server.py`. Full suite: identical failure set to main.

### Live checks

Two local OpenAI-compatible proxies: A (chat, key `proxy-key`) and B (embeddings, key `emb-key`), each logging the backend model it forwards to. The SciLink process held only the Bedrock key, so embeddings could not have gone direct.

- Chat direct to Bedrock, embedding base URL → B with `gemini-embedding-001`: no proxy chat traffic, one embedding call at B, correct retrieved answer.
- Chat through A, embeddings through B: chat calls only at A, embedding calls only at B.
- Main proxy only (regression): embeddings through A, the embedding key ignored with the warning, correct answer.
- Wrong embedding key: loud 401 at indexing. No embedding key: falls back to the main key with a warning.
- Store: `scilink kb create --base-url B` embedded through B; the meta's add-to-store through B.
- Web UI: the field appears with the embedding model; the hint switches from the vendor hint to the endpoint note; a Mission Control session started with the field sent it in the request, one add-to-store turn produced exactly one embedding call at B and nothing at A; after a server restart the session was resumed with the field and a second turn did the same.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa850f5686de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.