# Issue

**Title:** UI sweeps: do not embed the TEA grounding record as a report card

## Description

## Summary

.

After `run_economic_analysis`, the chat answer showed a report card titled "Tea Analysis.Grounding". That file is the TEA's evidence record: the retrieved knowledge-base chunks kept for the TEA critic and for later injection, not something written for you. It no longer appears as a document in either UI. Your actual deliverables are unaffected: the TEA report, technical documents with any filename, white papers and ideation reports still embed exactly as before, and a small unregistered note still surfaces so a forgotten flag cannot hide a file.

## Technical description

Both markdown sweeps embed any new `.md` that is either a registered deliverable or merely small and not a bulk stem. `tea_analysis.grounding.md` is deliberately never registered (the deliverables manifest marks only the TEA report), its stem is not a bulk stem, and it is under the inline size cap, so it passed the unmarked gates on both surfaces and got the prettified-stem title.

Change, scoped exactly as the issue's constraint asks:

- `scilink/server/artifacts.py` (React web app, the default UI): `_EVIDENCE_SUFFIXES = (".grounding.md",)`; `find_new_md_documents` skips a file with that suffix inside the `not is_marked` branch only.
- `scilink/ui/app.py` (Streamlit): the same rule inline in `_find_new_md_documents`, which the UI-contract tests exec standalone.
- A marked deliverable embeds regardless of name, size or suffix, so a technical document written with a custom `filename`, and even a marked file that happens to carry the suffix, are untouched. No title or stem matching was added.

Tests: `test_artifact_tracker_skips_evidence_record_but_embeds_marked_docs` (web) and the extended `test_ui_embeds_marked_deliverables_and_skips_bulk` (Streamlit) cover the unmarked evidence record, a marked custom-named document, a small unmarked note and a marked file carrying the suffix. The affected suites match main except for the one added test (the three `test_ui_reattach_apptest` failures are pre-existing on main).

Live: against `scilink serve` on loopback with a Bedrock plan session, a planted `tea_analysis.grounding.md`, a marked TEA report, a marked custom-named brief and an unmarked note; after a real turn the assistant message's `md_reports` held the three documents and not the grounding record.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f31994d55f6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.