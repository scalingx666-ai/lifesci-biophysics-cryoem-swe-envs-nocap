# Issue

**Title:** Web UI: reset the embedding preset when its option is no longer offered

## Description

## Summary

After manually picking the Bedrock embedder (Titan) and then switching the chat model to a non-Bedrock one, the Titan **option** was removed from the dropdown but the embedding-preset **state** stayed on Titan. The controlled select then showed `(none — keyword-only)` while the stale value still (a) rendered the "Embeddings use your Bedrock (AWS) credential" note and (b) was what the session request sent — so an OpenAI/Gemini chat session would silently send Titan embeddings while the UI showed none.

## Fix

`Sidebar.tsx`: an effect keeps `embeddingPreset` within the set of currently-offered options (`""`, `__custom__`, the config presets, and the Bedrock embedder only when the chat model is Bedrock). A value that is no longer offered falls back to `""` (keyword-only). The auto-select behavior (untouched: Bedrock → Titan, others → keyword) is unchanged.

### Verified in Chrome

Manual Titan on Bedrock → switch to OpenAI/Gemini: dropdown `(none)`, note gone, session sends `embedding_model: null`. Untouched Bedrock still auto-selects Titan with the note, and re-selects it on returning to Bedrock.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f67fd8134db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.