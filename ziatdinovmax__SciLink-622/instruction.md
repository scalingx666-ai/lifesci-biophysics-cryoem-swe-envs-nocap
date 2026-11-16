# Issue

**Title:** Web UI: main-model Base URL only for a Custom model

## Description

## Summary

The main model's **Base URL** is the chat-model proxy switch (the internal / OpenAI-compatible proxy that `SCILINK_API_KEY` + base URL authenticate against). Confirmed with the user that their proxy endpoints alias their models — Azure uses deployment names, LiteLLM / gateway proxies use aliases — so a proxy session always names the model itself (picks **Custom**). The Base URL field is now shown only for a Custom main model, consistent with the embedding base URL and the "Base URL = custom endpoint" model.

The earlier trade-off (a transparent proxy serving a model under its canonical/preset name would need Custom) does not apply to their deployments, which rename models.

## Technical description

- `Sidebar.tsx`: `baseUrlEff = shownModel === "__custom__" ? baseUrl.trim() : ""` is used by both `/api/v1/config` lookups and the session request, so a URL typed under Custom is never sent (or used for provider inference) once a preset is chosen. The Base URL field is gated on `shownModel === "__custom__"`.

### Verified in Chrome

Hidden for OpenAI and Bedrock presets; shown for Custom (with the custom model-name field); a Custom model + Base URL sends `base_url` in the session request; typing a Base URL under Custom then switching to a preset sends `base_url` empty.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f683ac81eab9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.