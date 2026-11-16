# Issue

**Title:** Chat loops: no outer retry on timeouts

## Description

> Stacked on #681 (the retry policy inside `litellm_completion` that makes this loop redundant). Review after #681.

## Summary

When a model call hung, the analysis, meta and simulation chat loops re-sent the same call up to three times on top of the retry already inside the LLM wrapper, spending the turn's iterations on it. A stuck call now fails after the wrapper's single retry.

## Technical description

**The defect.** The LiteLLM path of each chat loop wrapped `litellm_completion` in a try/except. It caught any error whose text said "timeout", printed "API timeout on iteration N. Retrying...", and `continue`d, up to iteration 3. With #681's policy underneath (one retry for a timeout), a stuck call cost up to six attempts.

**The change.** The outer try/except is removed from the LiteLLM path in `analysis_orchestrator.py`, `meta_orchestrator.py` and `simulation_orchestrator.py`. The timeout error now surfaces from `litellm_completion` after its own retry. The OpenAI-client (proxy) path is not covered by #681's policy and keeps its loop.

**Tests: `tests/test_chat_loop_timeouts.py`.** For each of the three orchestrators, a chat turn whose `litellm_completion` raises `Timeout` makes one call and surfaces the error. It fails on #681, where the loop re-sent it.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`). A meta chat turn with every real call forced to `timeout=0.5`:
- this branch: `Timeout` after **2 real calls** (7.5 s);
- #681 alone: **6 real calls** (9.5 s).

- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `13c02b9b4276`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.