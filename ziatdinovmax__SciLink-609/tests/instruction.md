# Issue

**Title:** Normalize tool schemas once, for every provider

## Description

.

## Summary

Plan/BO mode could not start on Gemini: one planning tool declared a parameter as a multi-type union with a shared `items`, Gemini rejected the declaration, and because Gemini validates all tools up front the whole request failed. The fix is provider-general, as the issue's comment asked: every tool schema is now normalized once, in the provider-abstraction layer, into the subset all providers accept, so every agent and mode is covered and tool authors keep writing lenient schemas. A test now runs every registered tool of all four orchestrators through the declaration rules, so a non-portable schema fails in CI instead of at request time on one provider.

Two related things surfaced and are handled in the same layer: LiteLLM silently drops `oneOf` parameters to an empty schema on Gemini (several analysis and simulation tools lost their typing there), and OpenAI's gpt-5 models refuse function tools on chat completions unless reasoning is turned off, which made tool calling on the listed OpenAI model fail outright.

## Technical description

- `scilink/wrappers/tool_schema.py`: `normalize_schema` (multi-type `type` → `anyOf` with type-scoped keywords: `items`/`minItems`… on arrays, `properties`/`required`… on objects, string/number keywords on theirs; `enum` split by variant; `null` as its own variant; `oneOf` → `anyOf`; recursive over properties, items, anyOf/allOf, additionalProperties; deep copy, never mutates). `normalize_tools` handles OpenAI-style and Google-style declaration lists. `gemini_schema_problems` / `tool_schema_problems` are the rules check.
- `litellm_wrapper.py`: `_build_params` normalizes `tools` (covers `generate_content` and the chat session) and sets `reasoning_effort="none"` for `openai/gpt-5*` when tools are passed and the caller set none (`_openai_tools_need_no_reasoning`); the module-level `litellm_completion` helper applies both. `openai_wrapper_tools.py` (internal proxy) normalizes the converted tools.
- Tests (`tests/test_tool_schema_portability.py`, 11): the normalizer rules; every registered tool of analysis, planning, simulation and meta passes the Gemini rules after normalization and survives LiteLLM's `_build_vertex_schema` with no `items` on a non-array variant and no parameter collapsed to `{}`; names, descriptions, required lists and property sets are preserved; the authored planning set is flagged by the check; the wrapper's reasoning rule per provider. Full suite: identical failure set to main.

### Live checks

- Each orchestrator's real tool set with a one-word prompt, through the wrapper: 12/12 accepted on Gemini (`gemini-3.8-flash`), OpenAI (`gpt-5.6-sol`) and Bedrock (Opus 4.8). Baseline before the fix: the planning set rejected by Gemini with the issue's exact error, and all four sets rejected by gpt-5.6-sol ("Function tools with reasoning_effort are not supported").
- End to end, autonomous: Bayesian optimization in plan mode on Gemini, OpenAI and Bedrock (`run_optimization` success, history written); the meta delegating the BO run on Gemini and OpenAI; analysis mode (single-spectrum fit) on Gemini, OpenAI and Bedrock; simulate mode (structure generation) on Gemini and OpenAI.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `edd01acc18ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.