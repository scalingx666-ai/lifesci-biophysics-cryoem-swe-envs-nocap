# Issue

**Title:** Meta: route literature-only questions to planning, not analysis

## Description

## Summary

If you ask the meta agent a question that only needs a literature review, with no data to analyse ("how novel is this approach?", "what's the prior art?"), it should give the question to the planning specialist. In one live session it gave it to the analysis specialist instead. The analysis specialist has no data to work on, so it can only run literature searches. It also started without the earlier planning results the question was about.

This PR changes two lines of prompt text so these questions go to planning. Planning already handles them well: an earlier novelty assessment in the same session went there and worked.

---

## Technical details

**Observed:** delegation 12 of a meta session was sent to `delegate_to_analysis` with the task "LITERATURE-BASED NOVELTY ASSESSMENT … no data file is involved". The analysis child ran only `search_literature` ×4 and `read_file` ×2, and never called `run_analysis`. No `context_from` was passed, so the planning deliverables it was assessing were missing, and the `thorough` depth profile had nothing to act on.

**Cause:** the `delegate_to_analysis` description lists "novelty checks" among the specialist's capabilities. Analysis-side novelty (`assess_novelty`) scores claims taken from an analysis record. The document-routing rules in the system prompt don't cover a literature-only question that comes with no documents, so that one word decided the route.

**Change:**
- `meta_orchestrator_tools.py`: "novelty checks" becomes "novelty of claims from an analysed dataset".
- `meta_orchestrator.py`: one routing sentence: a literature-only question (novelty, prior art, state of the field) with no data to analyse goes to `delegate_to_planning`.

**Testing:**
- Both modules import. No test asserts the old wording.
- `tests/test_mcp_meta_mode.py` passes on its own (6/6).
- The non-live meta tests: 123 passed, 1 failed. The failure is in `tests/test_mcp_meta_mode.py`, which passes on its own, so it looks order-dependent.
- There are 2 unrelated collection errors. The `tests/test_literature_no_overwrite.py` error also occurs without this change.
- Not re-run live yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `59279854a164`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.