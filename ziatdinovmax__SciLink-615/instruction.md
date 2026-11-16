# Issue

**Title:** Web UI: custom embedding model in the sidebar

## Description

## Summary

The React sidebar only offered the preset embedding models, so a different embedder could not be configured even though the backend accepts any name and the Streamlit sidebar already had a Custom entry. The embedding picker now has a Custom option with a name field, and the embedding API key hint follows the typed model's vendor (available from the matching environment variable, or leave blank to use the main key).

## Technical description

- `Sidebar.tsx`: `embeddingPreset` / `customEmbeddingModel` state; the effective `embeddingModel` (custom name when Custom is selected) is what `start`/`resume` send. A small effect refetches `/api/v1/config` with `embedding_model` to show `embedding_credential` under the key field.
- `api.ts`: `config(model, baseUrl, embeddingModel)`; `AppConfig.embedding_credential`.
- `app.py`: `get_config` takes `embedding_model` and returns `embedding_credential` via the existing `resolve_embedding_prefill` (availability only, never a value). `session_manager` logs the configured embedding model at session start.
- Tests (`tests/test_web_server.py`, 2 new): the endpoint for preset, custom and empty names; the custom name and key reaching `_init_meta_agent` and `_init_planning_agent` through `POST /api/v1/sessions`.

### Verified in Chrome

Custom entry and name field present; the hint reads "available from OPENAI_API_KEY" for an OpenAI-style name and the leave-blank note for an unknown vendor; a session started with `embedding_model: "voyage-3-large"` in the request body and the name shown locked in the running session's sidebar.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2bab837644a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.