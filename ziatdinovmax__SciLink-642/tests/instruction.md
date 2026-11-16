# Issue

**Title:** Documents: append the workflow diagram only when the body has none

## Description

## Summary

.

A technical document could show the same workflow figure twice: once where the author placed it in the body, and once more at the end where the workflow-diagram step appended its own rendering. The append step now checks first and leaves the document alone when it already carries a workflow figure. Documents without one still get the diagram exactly as before.

## Technical description

`_maybe_embed_workflow_diagram` (used by `write_technical_document` on new documents and by the white paper) gains an `_existing_workflow_figure` check that runs before any rendering, so a skipped append also saves the diagram-agent call. Three signals, any of which counts:

- a prior appended `## Campaign Workflow` section (a second pass over the same text);
- an embedded image whose alt text or filename contains "workflow" (the live case: `![Process and screening workflow…](mg_recovery_workflow.png)`);
- an embedded image byte-identical to a `campaign_workflow*.png` already in the output directory (a copy under an unrelated name).

Unrelated figures (a micrograph, a cost chart) do not block the append. The skip prints one line naming the figure it found. The revision path already skipped the append; the memo path never appends.

Tests: `tests/test_workflow_figure_not_duplicated.py` drives the helper with a stubbed diagram agent: inline workflow figure suppresses the append and the render; a byte-identical copy under another name counts; a document without one gets the section once and a second pass adds nothing; unrelated figures still get the diagram. The document, image-embedding and diagram-agent suites pass (39 tests).

No live run: the change is confined to the helper the tests drive directly, and whether the author embeds a figure inline is the model's choice, so a live document cannot be made to reproduce the duplicate on demand.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7837fd484828`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.