# Issue

**Title:** Web UI: the embedding key hint knows about the base URL

## Description

.

## Summary

With a base URL, SciLink sends every request through the proxy with the main API key: embeddings included, the embedding model id as typed, and any separate embedding key is ignored (the agents warn and drop it). The new embedding hint in the sidebar did not know that and could tell a proxy user that an environment key was available when it would never be used. The hint is now proxy-aware: with a base URL the key field gives way to a note saying embeddings go through the base URL with the main key and the model name is sent to the proxy as typed; clearing the base URL brings the key field and its vendor hint back.

## Technical description

- `app.py`: `get_config` returns `embedding_credential.proxied` and claims no vendor env var when `base_url` is given.
- `Sidebar.tsx`: renders the proxy note instead of the key field when `proxied`; `api.ts` type updated.
- Test: `embedding_credential` with a base URL is `{env_var: None, is_set: False, proxied: True}`; the existing preset/custom/empty cases carry `proxied: False`.

### Live checks

- A planning agent built with only the proxy key, a base URL to a local OpenAI-compatible proxy, the custom embedding id `text-embedding-3-small` and a knowledge directory: indexing and a retrieval query each hit the proxy's `/v1/embeddings` with that exact model id (the SciLink process had no vendor key), the ignored-key warning fired, and the top hit was the right chunk.
- In Chrome: with a custom OpenAI-style name the hint reads "available from OPENAI_API_KEY"; typing a base URL swaps the key field for the proxy note; clearing it restores the field and hint.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d9478e7c348`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.