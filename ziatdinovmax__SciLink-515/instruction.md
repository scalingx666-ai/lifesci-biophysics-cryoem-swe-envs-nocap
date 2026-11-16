# Issue

**Title:** Lock the run_task cap-exhaustion status flip; document the meta cap split

## Description

## Summary

Two small follow-ups left over from the #208 fix (which stopped `run_task` from reporting a gave-up-mid-task turn as success). First, that fix now has a tripwire: regression tests on all three orchestrators assert that a chat turn exhausting its iteration cap surfaces from `run_task` as `status="error"` — the signal crosses the chat-loop/`run_task` seam via a flag, so a future refactor could silently reintroduce the old bug and nothing would have caught it. Second, the meta agent's `max_iterations` is now documented honestly: it caps the meta's own chat loop only and is not passed down to specialist children, who keep their own default of 20 — previously the parameter was undocumented and it was natural to assume it applied everywhere.

.

Per the issue's own recommendation, the alternative "plumb the meta's cap into delegations" option is deliberately not taken here — that's an API-shape change worth its own discussion, and the live test confirms the current split works as now documented.

---

**Technical details**

- `tests/test_run_task_cap_exhaustion.py` (6 tests): for analysis / planning / simulation, a stubbed `chat()` that sets `_last_chat_hit_iter_cap` and returns the warning string must yield `run_task` → `status="error"` with the warning in `error`, and the per-call `max_iterations` override must be restored on the instance; clean-completion controls pin the success path. Mirrors the `test_5_run_task_without_llm` stub shape.
- `MetaOrchestratorAgent` class docstring gains the `max_iterations` entry naming the split (meta's own loop only; delegation depth vs per-delegation tool budget are separate dimensions; an exhausted child reports the delegation as an error).
- Live (Bedrock opus-4-8), 9/9: a REAL cap exhaustion (`run_task(max_iterations=2)` on a genuine fitting task) returns `status="error"` with the cap warning and restores the cap; the same task under the default cap succeeds with one analysis; a meta built with `max_iterations=50` delegates while its analysis child verifiably runs under its own cap of 20, and the delegation succeeds (fitted peak center 5.100 ± 0.003 vs planted 5.1).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ecafe7cf3b3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.