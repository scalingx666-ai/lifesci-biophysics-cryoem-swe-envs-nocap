# Issue

**Title:** Turn off the scalarizer's per-file column-confirmation prompt

## Description

## Summary

Ingesting a data file no longer stops to ask you to confirm the extracted columns. That prompt appeared only when the scalarizer had to generate extraction code, after its own automated checks had already passed, and in co-pilot or autopilot it could stall a delegated run waiting for a keypress. Reviewing which columns are the inputs and the target is still worth doing, but at the campaign level, not once per file. Nothing changes for autonomous runs, which never showed the prompt.

## Technical description

- `orchestrator_tools.analyze_file`: both `scalarize` calls (initial and the sidecar-mismatch regeneration) pass `enable_human_review=False`; `analyze_batch` already did. The autonomy-level gate `_get_human_feedback_enabled` is unchanged and still governs plan and code review elsewhere.
- `scalarizer_agent.scalarize`: default `enable_human_review` flips to `False`; docstring says why. The review block itself stays for an explicit opt-in caller.
- `scilink/ui/app.py` and `scilink/server/presenter.py` keep their `review_metrics` handling; it is now dead unless a caller opts in.
- `tests/test_scalarizer_review_off.py`: the scalarizer default is off, and `analyze_file` and `analyze_batch` pass `enable_human_review=False` in all three autonomy levels (captured through a stubbed `scalarize`).
- Live check on Bedrock `us.anthropic.claude-opus-4-8`: an autopilot orchestrator (orchestrator-level human feedback on) ingested a raw two-column spectrum through the codegen path with stdin closed; one attempt, auto-reflection passed, no review prompt, correct peak value stored.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78704328ded5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.