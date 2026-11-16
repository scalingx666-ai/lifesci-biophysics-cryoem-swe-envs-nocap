# Issue

**Title:** Judge completion against the request as stated

## Description

.

## Summary

An analysis self-review could upgrade the user's ask into a stricter deliverable it was never given (a phrase describing a result's granularity read as a demand for a table of it), judge its own correct output insufficient, and then run unrequested follow-up analyses to satisfy the phantom requirement. Both the analysis orchestrator and the meta now carry the principle as a single behavioral rule: judge completion against the request as stated, never invent a deliverable, format or acceptance criterion beyond it, and produce an extra artifact only when explicitly requested.

## Technical description

- `analysis_orchestrator.py`: first item of the **BEHAVIOR** list in `_SYSTEM_PROMPT_BODY_POST`.
- `meta_orchestrator.py`: first item of **RESPONSE STYLE** in the meta prompt, phrased for the reviewer of a specialist's result (no follow-up delegations to satisfy an invented criterion).
- One sentence each, stating the principle rather than enumerating output types, per the prompt-patch convention. Test: `tests/test_verify_against_request.py` pins both.

A live check of the trigger scenario is reported with the companion PR for #599, which adds the lightweight derive step the chase mis-routed through.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bce5f879707`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.