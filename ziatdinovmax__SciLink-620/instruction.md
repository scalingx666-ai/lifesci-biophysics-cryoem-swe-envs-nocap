# Issue

**Title:** Web UI: hide the embedding base URL / key fields for Bedrock embedders

## Description

Small UX follow-up to the Bedrock embedding auto-select (#619).

## Summary

An embedding **base URL** (an OpenAI-compatible endpoint) and a separate embedding **API key** don't apply to a Bedrock embedder: Titan (the Bedrock auto-default) is reached through the same AWS credential as the chat model, not through an endpoint. Those two fields are now hidden for any `bedrock/` embedding model and replaced with a one-line note. They stay for custom and OpenAI/Google models — which is where the base URL is actually most useful (a self-hosted or proxied OpenAI-compatible embedding service).

## Technical description

- `Sidebar.tsx`: the base URL field and the embedding-API-key field are gated on `!isBedrock(embeddingModel)`; a `bedrock/` embedder instead shows the caption "Embeddings use your Bedrock (AWS) credential — no separate key or endpoint needed." Custom / vendor models are unchanged.

### Verified in Chrome

Bedrock chat (Titan auto-selected): only the embedding-model dropdown plus the AWS-credential note — no base URL, no key field. OpenAI preset (`text-embedding-3-small`) and Custom (`voyage-3-large`): both show the base URL and key fields.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `95fec2f545e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.