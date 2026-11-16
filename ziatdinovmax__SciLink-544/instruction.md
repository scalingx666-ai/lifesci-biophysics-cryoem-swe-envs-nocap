# Issue

**Title:** Files-tab markdown listing newest-first; un-hardcode a test's repo path

## Description

## Summary

Two small cleanups.

- In the web UI's Files tab and the end-of-turn document cards, markdown documents produced in a turn are now listed newest first. Before, the order was whatever the filesystem walk returned, so the document written last could appear at the bottom.
- `tests/test_session_title.py` no longer hardcodes a path from another machine, so it runs on any checkout instead of failing at collection.

## Technical description

- `scilink/server/artifacts.py`: `find_new_md_documents` sorts its result by file mtime, descending, after the existing filtering (bulk stems, size cap, deliverable marking). No change to which files are included. This is the follow-up the #533 issue text named as a separate, lower-priority surface.
- `tests/test_session_title.py`: derives `_REPO` from `Path(__file__).resolve().parents[1]` for `sys.path` and for the two source files it inspects (`sidebar.py`, `app.py`).
- New test `test_artifact_tracker_md_newest_first` in `tests/test_web_server.py` creates three documents with staged mtimes across a subdirectory and asserts the order.

Not changed: several `*_live.py` tests and `test_read_file_offset_and_index.py` also reference session data from another machine; those depend on data that does not exist elsewhere, not only on a path, and are outside this fix.

Verified: `test_session_title.py`, `test_web_server.py` (40 tests), and `test_artifact_image_rewrite.py` pass.

Also: the script-style test ended with a module-level `sys.exit`, which under pytest aborts the whole collection run once the file imports cleanly. It is now guarded behind `__main__` and exposed through a `test_session_title_checks` function, so it runs as a normal test (43 pass across the three touched files) and still works as a script.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b74a17eca46d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.