# Issue

**Title:** Plan refinement: log banners say "Results / Feedback"

## Description

## Summary

.

When a plan is refined from your feedback or a critique rather than from experimental results, the log no longer announces it as "Refining Plan based on Results". Both banners now read "Results / Feedback", matching the HTML report box that was already relabelled.

## Technical description

Two print banners on the `refine_plan` channel changed, nothing else:

- `planning_agent.py`: `--- 🔄 Refining Plan based on New Results / Feedback ---`
- `orchestrator_tools.py`: `⚡ Tool: Refining Plan based on Results / Feedback...`

Tool descriptions and docstrings are untouched, as the issue asks: they describe the tool's primary purpose to the LLM, not the user-facing log. The other refine banners (`📝 Refining plan...`, image/curve controllers) were already neutral.

Test: `tests/test_refine_banners_label_feedback.py` pins the wording on all three surfaces (both banners and the HTML label) and asserts the results-only banners are gone. A live run is not needed for a two-string change; the banner path is exercised by every refinement.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7837fd484828`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.