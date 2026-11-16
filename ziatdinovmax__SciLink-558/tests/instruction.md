# Issue

**Title:** Meta: read_file — the plain reader it lacked (#397's last phase)

## Description

## Summary

The mission-control agent can now read an uploaded script or any plain text file itself. Stacked on #556.

Observed in your live session today: handed an attached `.py`, the meta probed the first lines, tried `view_document` (which refuses code), tried a session-history search, and routed from the probe alone. It routed correctly, but it could not read what you gave it. Now it reads the file with `read_file`, the same reader every specialist has, with the same navigation for long files.

## Technical description

- `scilink/agents/meta_agent/meta_orchestrator_tools.py`: `read_file(file_path, max_lines, tail, search, offset)` registered after `view_document`, a thin wrapper over `scilink.utils.file_io.read_file_content` (the #481 engine) with whole-read stems `report`, `summary`, `white_paper`, `literature`; relative paths resolve against the meta session directory. `view_document`'s refusal for a non-document extension now ends with "for a script, log, config or any other text file use read_file".
- `scilink/agents/meta_agent/meta_orchestrator.py`: one sentence in the inspecting-uploads block saying which reader is for what.
- Test (`tests/test_meta_inspect_subfolders.py`): the tool registers with its parameters, reads a 301-line script relative to the session with truncation, finds a symbol with `search` at the right line, reads the tail, returns a report whole, errors on a missing file, and `view_document` now points at `read_file`.

Live (Bedrock Opus 4.8): a meta session read the attached `user_edge_fit.py` with `read_file` twice — once for the summary, once with `search` — and answered the line-number question correctly (line 31).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b630ab7013c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.