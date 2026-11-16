# Issue

**Title:** LLM calls: retry what a retry can fix, with backoff, and time out

## Description

## Summary

When the model provider throttles SciLink or is briefly unavailable, SciLink now waits before trying again, and waits a little differently each time. Before this change, LiteLLM retried at once with no wait at all: a throttled Bedrock account was hit four more times in the same second, and every agent running in parallel came back at the same moment. Errors that no retry can fix (a malformed request, bad credentials) are now reported straight away instead of being sent four more times. Every model call also has a time limit, so a stalled connection can no longer hang a session for up to 100 minutes.

Nothing changes when the provider is healthy. This is stage 0 of the agent-swarms proposal (#676), which needs it before many agents share one provider.

## Technical description

**The defect.** `litellm_completion` (every orchestrator chat loop) and `LiteLLMGenerativeModel.generate_content` (every controller) passed `num_retries=4` to LiteLLM, and the comments said this retried "with LiteLLM's exponential backoff". LiteLLM 1.81's `completion_with_retries` defaults to `retry_strategy="constant_retry"`, which is `tenacity.Retrying(stop=stop_after_attempt(n))`, with no wait. The `client` wrapper also routes every `openai.APIError` into it, `BadRequestError` and `AuthenticationError` included. `LiteLLMChatSession.send_message` had neither retries nor a timeout, and `litellm_completion` set no timeout, so LiteLLM's `request_timeout` of 6000 s applied.

**The change.** All in `scilink/wrappers/litellm_wrapper.py`.
- **One retry loop.** `_completion_with_retries(retries, **kwargs)` now runs every call on the three paths. It passes `num_retries=0` to LiteLLM, so there is exactly one retry loop.
- **What is retried.** `_is_transient` retries `RateLimitError`, `ServiceUnavailableError`, `InternalServerError`, `BadGatewayError`, `Timeout` and `APIConnectionError`, plus any error whose `status_code` is 408, 429, 500, 502, 503, 504 or 529.
- **Backoff.** `_backoff_s` is exponential with equal jitter: step = min(60, 2·2^attempt), and the wait is step/2 + U(0, step/2). `Retry-After` is honoured, capped at 60 s.
- **Timeouts.** A `Timeout` is retried once, not the full budget, because a generation longer than the timeout would only time out again.
- **Logging.** A warning is logged before each wait. Under the web and shell capture that log write raises a pending Stop, so a stopped turn does not sit through its backoff.
- **Timeout defaults.** `litellm_completion` now defaults `timeout` to `LLM_TIMEOUT_S = 1200`, the generative wrapper's existing default. The chat session passes its model's timeout. An explicit caller `num_retries` or `timeout` still wins.
- **Positional arguments.** `litellm_completion` still accepts `model` (and `messages`) positionally.

**Tests.**
- `tests/test_llm_retries.py`:
  - transient errors are retried with waits between;
  - LiteLLM gets `num_retries=0` and every attempt gets a timeout;
  - 400 and 401 are raised at once;
  - `Retry-After` is honoured;
  - backoff grows, stays inside its bounds and is not in lockstep;
  - the budget is bounded and the last error surfaces;
  - a timeout is retried once;
  - a caller can turn retries off or set its own timeout;
  - `generate_content` and the chat session use the same policy.
- `test_retries_still_defaulted` in `tests/test_litellm_completion_max_tokens.py` asserted the old `num_retries=4` pass-through. It becomes `test_retries_are_scilinks_not_litellms`.

**Not changed.**
- `OpenAIAsGenerativeModel` (the proxy path) keeps the OpenAI client's own retries, which do back off.
- The embedding path keeps its batch-splitting retry.
- There is no cross-process rate limiter yet: that is stage 1 of the proposal.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`):

| check | attempts | result |
|---|---|---|
| `litellm_completion`, normal call | 1 | success; sent `timeout=1200`, `num_retries=0` |
| `generate_content`, normal call | 1 | success (`'OK'`) |
| chat session `send_message` | 1 | success; sent the model's `timeout=1200` (had none) |
| unknown model id (`BadRequestError`) | 1 | raised in 0.6 s; **on `main` the same call made 5 HTTP requests in 2.0 s** |
| forced timeout (`timeout=0.5`) | 2 | `Timeout` raised after one retry |
| a planning orchestrator chat turn | — | normal reply in 4.4 s |
- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8f8560dd7ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.