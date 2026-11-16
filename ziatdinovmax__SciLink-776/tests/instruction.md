# Issue

**Title:** Drop a sampling parameter a model refuses, and remember the model

## Description

## Summary

Newer models from several providers no longer accept the "sampling" settings (`temperature`, `top_p`, `top_k`). A request that sets one is rejected outright. SciLink sets `temperature: 0.0` in a few places, most visibly the image analysis verifier. On such a model the verifier's call failed and was scored 0.0, every candidate failed the quality gate, and the result was reported unverified. Anthropic's Claude 5 models (Fable 5.1, Sonnet 5.5, Haiku 5.5) and OpenAI's GPT-6 already reject these settings, and Google has announced the same for its upcoming Gemini models.

With this change, SciLink notices such a rejection, sends the request again without these settings, and stops sending them to that model for the rest of the session. Models that still accept the settings see exactly the same requests as before.

---

## Technical details

**The rule** (`scilink/wrappers/litellm_wrapper.py`, `call_dropping_refused_sampling`)

- It sits at the two places every completion request already passes:
  - `_completion_with_retries`, behind `LiteLLMGenerativeModel.generate_content`, `LiteLLMChatSession.send_message` and `litellm_completion`;
  - the OpenAI-compatible client's `_PortableCompletions.create` (`tool_schema.py`), behind `OpenAIAsGenerativeModel` and `OpenAIAsGenerativeModelWithTools`.
- A refusal is a 400 (the HTTP status anywhere down the exception chain, or LiteLLM's `BadRequestError`) whose message names a sampling parameter together with a refusal word: deprecated, not supported, unsupported, does not support, not allowed, only the default, no longer supported.
- On a refusal, the request is sent once more without the sampling parameters, and the model name is added to a process-level set. Later requests to that model omit the parameters up front. A warning is logged the first time.
- Raised as before:
  - any other 400 (e.g. "temperature must be between 0 and 1", "prompt is too long");
  - a refusal on a request that set no sampling parameter;
  - a refusal wording on a non-400 status.
- The existing name rules (`_model_deprecates_sampling` for Opus ≥ 4.8, `_is_openai_reasoning_model`) are unchanged and remain the fast path.

**Tests** (`tests/test_sampling_refusal.py`, 14)

- Real HTTP against a local server that answers Anthropic messages and OpenAI chat completions, refusing sampling parameters the way the providers do:
  - a refused `temperature` is dropped, and the model is remembered (the next request is a single one without it);
  - a model that accepts `temperature` gets the same single request each time, and nothing is learned;
  - another 400 is raised, with no resend;
  - the OpenAI-compatible client path behaves the same.
- Unit tests of the refusal wordings: Anthropic, OpenAI "Unsupported value / parameter", generic, and the wordings that must not match.
- With the fallback disabled, exactly the two path tests fail.

**Verification**

- **Live, `temperature: 0.0`, two calls per model, this branch against `main`:**

  | Model | `main` | This branch |
  |---|---|---|
  | Bedrock `claude-fable-5-1`, `claude-sonnet-5-5`, `claude-haiku-5-5`; `gpt-6-sol` (direct, and through an OpenAI-compatible endpoint) | 400 on every call | success: one extra request the first time, none after |
  | `claude-sonnet-4-5`, `gpt-4.1`, `gemini-3.8-flash` (direct and OpenAI-compatible) | `temperature` sent, one request | identical |
  | `claude-opus-4-8`, `gpt-5.5` | already omitted by the name rules | identical |

- **End to end:** an image analysis on Bedrock `claude-fable-5-1` got one refusal, then a verifier score of 0.87, giving `verified: true` (`decided_by: qc_gate`). On `main` the same verifier call returns the 400 and scores 0.0.
- **Full offline suite against a `main` worktree, by failing-test id:** identical (42 failed / 32 errors on both), with 14 additional passes from the new tests.

**Not in this PR**

- A verifier call that fails on an API error is still scored 0.0, indistinguishable from a bad result. It should be reported as "verification unavailable". That changes best-of-N selection and labelling, so it needs its own A/B.
- `gpt-6*` is not matched by the OpenAI reasoning-model name rule, so a caller that sets `max_output_tokens` gets "use 'max_completion_tokens'". No SciLink caller sets it today.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc059709d7a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.