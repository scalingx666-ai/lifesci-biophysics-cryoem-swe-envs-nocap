# Issue

**Title:** Web UI: auto-select Titan embeddings on Bedrock; trim the keyword-only caption

## Description

Follow-ups to the no-model embedding default (#618).

## Summary

On Amazon Bedrock, the same AWS credential that runs the chat model also covers a first-party embedder, so dense retrieval is essentially free there. Selecting a Bedrock chat model now **auto-selects `amazon.titan-embed-text-v2:0`** for embeddings (the user can change it, and a manual choice sticks); other providers keep the keyword-only default, since their embedder needs a separate key. Two small text fixes: the keyword-only caption is trimmed, and it no longer shows for the Custom option.

Titan Text Embeddings V2 is the right auto-default: first-party, no separate Bedrock model-access opt-in (unlike Cohere), no doc/query `input_type` nuance, and — verified live — it builds and queries a knowledge base (1024-dim dense index, correct retrieval) with only the Bedrock credential.

## Technical description

- `Sidebar.tsx`: `BEDROCK_EMBED = "bedrock/amazon.titan-embed-text-v2:0"`; an effect sets the embedding preset from the chat provider (Bedrock → Titan, else none) until `embeddingUserSet` flips on the first manual change; the Titan row is injected into the options only on Bedrock sessions. The keyword-only caption is trimmed and gated on `!embeddingModel && embeddingPreset !== "__custom__"`.
- No backend change: `bedrock/amazon.titan-embed-text-v2:0` flows through the existing embedding-model plumbing; the config endpoint maps it to the AWS credential (`env_var: AWS_BEARER_TOKEN_BEDROCK`), so the key hint reads honestly.

### Verified

- Live (KnowledgeBase, only the Bedrock key): `bedrock/amazon.titan-embed-text-v2:0` builds a dense 1024-dim index and retrieves the correct chunk. `bedrock/cohere.embed-english-v3` also works (the alternative for users who enable Cohere access).
- Chrome: Bedrock chat auto-selects Titan; the session request carries it and the backend starts cleanly; switching to OpenAI reverts to keyword-only; a user override sticks across model switches; the caption is trimmed and hidden for Custom.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d578d4d1fa05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.