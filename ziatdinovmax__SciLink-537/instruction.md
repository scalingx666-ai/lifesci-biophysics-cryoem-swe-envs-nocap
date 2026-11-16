# Issue

**Title:** Resume card surfaces the headline deliverable, not the newest file

## Description

## Summary

. When you resume a session, the "Latest deliverable" card now shows the session's headline document, not simply whichever file was written last.

Before, a session that produced a white paper and then refined its ideation report or portfolio came back showing the refined ideation report, because that file had the newer timestamp. Now the card prefers the white paper, then any other document (technical document, memo, roadmap, plan report, a file you named yourself), and only then the ideation report or portfolio. Within the same kind, the newest still wins, so a revised white paper replaces the earlier one. Older versions remain in the Files tab as before.

Applies to both the web UI and the Streamlit app.

## Technical description

### Root cause

`collect_restored_deliverables` (web server, feeds the React card) and `_collect_restored_deliverables` (Streamlit sidebar) both selected `max(mtime)` over manifest entries marked `deliverable: true`. A late delegation that rewrites `ideation_report.md` without touching `white_paper.md` wins on mtime.

### Change

- `scilink/agents/planning_agents/user_interface.py` (streamlit-free, already the shared home of `record_deliverable` / `load_deliverables`): new `deliverable_rank(entry)` and `select_headline_deliverable(entries)`.
  - Rank 0: stem starts with `white_paper`, or title contains "white paper".
  - Rank 2: stem starts with `ideation_report` / `portfolio`, or title contains "ideation report", "portfolio", "candidate directions".
  - Rank 1: everything else.
  - Classification uses both the filename stem and the recorded title because the manifest carries no kind field and user-chosen filenames are common (`top3_brief.md` titled "White Paper v2" ranks 0).
  - Selection key is `(rank, -mtime)`; only existing `.md` / `.html` / `.htm` files qualify (PDF and DOCX twins are recorded but not embeddable); unmarked entries and missing files are skipped; stat failures are skipped rather than raised.
- `scilink/server/session_manager.py`: `collect_restored_deliverables` delegates to the ranker; return shape `(md_paths, html_paths)` unchanged, so `SessionManager.resume` and the React card are untouched.
- `scilink/ui/components/sidebar.py`: `_collect_restored_deliverables` delegates to the same ranker; comment updated to record the reason.

Because both selectors call one function, the two UIs cannot drift apart again.

### Tests

`tests/test_deliverable_card_priority.py` (new, 7 tests): white paper beats a later-refined ideation report; a session with only secondary artifacts still surfaces one; newest wins within a rank; other documents outrank working artifacts and a white paper outranks those regardless of age; unmarked, missing, and PDF entries are skipped while an HTML deliverable lands in the HTML slot; title-based ranking for user-chosen filenames; manifests pooled across child orchestrators (`<meta>/planning/...`). Related suites (`test_web_server`, `test_workspace_listing`, `test_ideation_campaign_artifacts`, `test_technical_document_tool`, `test_campaign_scoping`, `test_rename_copy_delegation_integrity`, `test_md_to_pdf`): 144 pass. The Streamlit file is compile-checked; it cannot be imported in the test env (no `streamlit`).

### Not changed

The Files-tab markdown listing order (`scilink/server/artifacts.py`, `find_new_md_documents`) walks the filesystem rather than sorting by recency, as the issue notes. That is a separate surface and is left for a follow-up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `78704328ded5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.