# Issue

**Title:** Web UI: embedding base URL only for Custom; reveal it on selection; honest key hint

## Description

Follow-ups from testing the embedding picker.

## Summary

- **Base URL only for Custom.** The embedding base URL is inherently a custom-endpoint setting, so it now appears only when the embedding model is **Custom** — not for a vendor preset (`gemini-embedding-001`, `text-embedding-3-*`) or a Bedrock embedder. To use a vendor model at a non-standard endpoint, choose Custom and type the name + URL. A URL typed under Custom is not sent once a preset is selected.
- **Reveal base URL + key on Custom selection.** They previously stayed hidden until a model name was typed, so the base URL looked missing for Custom; they now show as soon as Custom is chosen.
- **Honest key hint on Bedrock.** With a Bedrock chat model and a non-Bedrock embedder, the fallback hint no longer implies the main (Bedrock/AWS) credential covers it — it tells the user to set an embedding key or base URL.

## Technical description

- `Sidebar.tsx`: `embeddingBaseUrlEff` = the base URL only when `embeddingPreset === "__custom__"` (used by the config fetch and the session request, so a stale value is never sent). The base URL field is gated on `embeddingPreset === "__custom__" && !isBedrock(embeddingModel)`; the base URL and key fields use `embeddingActive` (a model chosen OR Custom selected) so they appear immediately on Custom. The fallback key caption branches on `isBedrock(effectiveModel)`.

### Verified in Chrome

Gemini / OpenAI presets: key field only, no base URL. Custom: model name + base URL + key, shown on selection. Bedrock (Titan auto): model dropdown + the AWS-credential note only. Custom embedder on a Bedrock main model: "Set an embedding API key or a base URL — your Bedrock (AWS) credential cannot be used for this embedder."

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e5e367e9f05`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.