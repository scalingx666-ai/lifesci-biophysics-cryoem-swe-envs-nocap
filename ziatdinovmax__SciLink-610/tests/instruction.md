# Issue

**Title:** Portable tool schemas on the internal-proxy path too

## Description

(issue #606).

## Summary

The first fix normalized tool schemas in the LiteLLM wrapper, but the internal-proxy path (a `base_url` pointing at an OpenAI-compatible proxy) never goes through it: each orchestrator's proxy chat loop builds its own OpenAI client and sends the authored schemas as written. A proxy in front of Gemini therefore still rejected plan/BO mode, and a proxy in front of a gpt-5 model refused function tools everywhere. Every proxy-path client is now built through one wrapper that applies the same normalization and the same gpt-5 reasoning rule, so both paths send identical, portable declarations.

## Technical description

- `tool_schema.portable_openai_client(raw, default_model)` → `PortableOpenAIClient`: `chat.completions.create` normalizes `tools` and sets `reasoning_effort="none"` for gpt-5 models when tools are present and the caller gave no value; all other attributes delegate to the wrapped client; wrapping is idempotent. `openai_tools_need_no_reasoning` moves here (accepts bare proxy model ids and LiteLLM-prefixed names) and the LiteLLM wrapper imports it.
- Wired at every proxy-path client: the analysis, planning, meta and simulation chat loops, the structure agent's resolver loop, and both `OpenAIAsGenerativeModel` wrappers. The analysis loop's empty-answer follow-up referenced `self.client`, which was never assigned; it is now the same portable client.
- Tests (2 new, 13 total in `tests/test_tool_schema_portability.py`): the wrapper on a fake client, and a static check that no proxy call site builds a raw client. Full suite: identical failure set to main.

### Live checks through a local OpenAI-compatible proxy

A minimal FastAPI proxy forwarding `/v1/chat/completions` verbatim to Gemini (`gemini-3.8-flash`), OpenAI (`gpt-5.6-sol`) and Bedrock (Opus 4.8).

- Control with the raw SDK, each orchestrator's real tool set: the planning set rejected by Gemini with the #606 error, every set rejected by the gpt-5 model. With the wrapper: 12/12 accepted.
- End to end on the proxy path, autonomous: BO in plan mode to Gemini, OpenAI and Bedrock (`run_optimization` success, history written); the meta delegating BO to Gemini; analysis mode to Gemini and OpenAI; simulate mode to Gemini.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dac8637c372`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.