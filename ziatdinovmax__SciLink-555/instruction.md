# Issue

**Title:** Shared file I/O engine; analysis mode gets read / append / edit / rename

## Description

## Summary

. All three modes now share one implementation of the everyday file tools, and analysis mode gets the full set it was missing.

- **Analysis mode can now read, append, edit, and rename session files.** Before it could only save a file and read a document. Now, like planning and simulation, it has `read_file` (with offset, tail, and search for long files), `append_file` for chunked writes, `edit_file` for exact snippet swaps with a backup, and `rename_file` for byte-exact renames. `save_file` and `read_document` keep working as before.
- **Two safety guards, everywhere at once.** Overwriting an existing file with `save_file` now keeps a `.before_overwrite` copy (counter-suffixed, never clobbered), the way `edit_file` always kept a `.before_edit` copy. And reading a large JSON file no longer dumps the whole thing into the agent's context: it is paged like any other text, with search to jump to a key.
- **Nothing else changes for the agents.** Planning still records deliverables and refreshes PDF twins on save; simulation still returns reports and summaries whole; the transcripts read the same. The three modes cannot drift apart again, because there is one reader and one writer.

Verified live on Bedrock Opus 4.8 in all four agents (analysis, planning, simulation, and a meta delegation), plus an adversarial sweep of every tool in every mode. Details below.

## Technical description

### Engine: `scilink/utils/file_io.py` (new)

The `file_edit.py` pattern: one implementation per operation, everything mode-specific as a parameter.

- `read_file_content(path, *, max_lines, tail, search, offset, full_read_stems, full_read_max_chars, ocr_model, text_cap_mb, doc_cap_mb, display_path)`: size guards (5 MB text, 25 MB documents, none for tabular), CSV/Excel preview (rows × cols, adaptive char budget), PDF/DOCX through `parsers.extract_text` with OCR fallback, JSON pretty-printed and windowed, everything else UTF-8 text. `window_lines(...)` is the windowing half on its own: search (case-insensitive regex, 40-hit cap, one line of context), offset, tail, head, the truncation notice that names the way out, and the section outline (two or more headings).
- `write_text_file(base_dir, filename, content, *, subfolder, append, backup_on_overwrite)`: traversal-proof (last component of filename and subfolder only), mkdir, append or overwrite; on overwrite copies the previous content to `<stem>.before_overwrite<suffix>` then `.before_overwrite.2`, `.3`, …; a backup failure is logged and the write proceeds (a lost backup beats a lost write). Returns `path`, `size_bytes`, `created`, `overwritten`, `backup`.
- `extract_document_text(path, ocr_model, max_chars)` and `read_documents_combined(paths, *, base_dir, ocr_model, max_chars)`: the extraction, combine, cap, per-document metadata and OCR note that analysis and simulation each carried.

### Wrappers

- **Planning** (`orchestrator_tools.py`): `save_file` and `append_file` call the engine rooted at `_output_dir()` (delegation scoping preserved), then layer deliverable recording and the PDF-twin refresh; the overwrite backup is recorded as a produced non-deliverable file, matching how `edit_file` records its pre-edit copy. `read_file` keeps `_resolve_data_path` and its `("literature_search",)` whole-read stems and calls the engine. About 270 lines of duplicated body removed.
- **Simulation** (`simulation_orchestrator_tools.py`): `save_file`, `append_file`, `read_file` (stems `report`, `summary`, `literature_search`) and `read_document` on the engine; the module-level `_extract_document_text` is kept as a name and delegates. About 350 lines removed.
- **Analysis** (`analysis_orchestrator_tools.py`): `save_file` on the engine (gains the backup); `read_document` on the engine with the literature-file persistence and `run_analysis` hint kept, and relative paths now resolve against the session dir; **new** `append_file`, `read_file` (stems `report`, `summary`, `literature`), `edit_file` and `rename_file` as thin wrappers over the engine and `utils/file_edit`, with analysis-flavored routing text (a snippet miss points at `read_file`; an oversized edit points at `save_file`, which now backs up). The analysis orchestrator prompt gains a "SESSION FILES" item stating the routing principle in one paragraph.

Per-mode routing text and tool descriptions are otherwise preserved verbatim.

### One more fix the live test forced

The meta's specialist-capability inventory is cached on disk keyed by package version and extensions. In a checkout or editable install the version does not move between releases, so after this change the meta still told the user the analysis specialist had only `save_file` and `read_document` (observed live; delegation itself worked). The key now also includes a content hash of the three tool-registration modules (`_TOOL_SOURCE_MODULES`, located via `find_spec` without importing, so a missing `ase` never matters). Re-running the same question live after the fix listed all six tools.

### Tests

- `tests/test_file_io_engine.py` (new, 11): windowing modes and notices, whole-read stems and their cap, missing and oversized files, small JSON returned whole and large JSON windowed and searchable, CSV preview, PDF extraction and windowing, combined documents with relative paths and errors, append and counter-suffixed backups with opt-out, the outline.
- `tests/test_file_tools_per_mode.py` (new, 6): the registered tools of each mode against a bare orchestrator: analysis' full surface and its roundtrip (save, append, read, edit with backup and the `read_file` hint on a miss, overwrite backup, rename with escapes stripped), its windowing and JSON cap on `analysis_results.json`, `read_document` persisting the literature file; simulation's backup, whole-read stems and no literature persistence; planning's deliverable recording, backup, and the backup's manifest entry.
- `tests/test_capabilities_cache.py`: +1, the key moves when a tool module's source changes and tolerates a missing module.
- Pre-existing `test_read_file_modes.py`, `test_save_file_chunking.py`, `test_edit_file_tool.py` pass unchanged (one test in `test_save_file_chunking.py` fails on `main` too, for an unrelated dispatch-guard message).
- Full suite on the branch versus a clean `main` worktree: identical failure sets except `test_trim_gate.py::test_no_hardcoded_gate_literal`, which passes on its own and as a file on the branch (order-dependent, untouched by this change); the branch passes 16 more tests than `main`.

### Live verification (Bedrock Opus 4.8, autonomous)

- **Analysis**, 6 turns: `read_document` persisted the literature file and `examine_data` ran; `save_file` then `edit_file` then `read_file` roundtrip with the `.before_edit` copy on disk; `read_file` search found the Conclusions section of a 454-line report at line 452 without a full read; `read_file` search found `entry_599` in a 600-entry JSON at line 4794; overwrite reported `session_notes.before_overwrite.md` and `rename_file` did a true rename; `run_analysis` on the synthetic spectrum then `read_file` on the produced `analysis_results.json` reported centers 420.03 and 609.94 against ground truth 420 and 610.
- **Planning**, 4 turns: PDF read through the extractor answered the mode at 610; a 114-line deliverable written as `save_file` plus five `append_file` chunks; `edit_file` replaced three occurrences with a backup and `read_file` tail showed the end; overwrite kept `protocol.before_overwrite.md` and reported the PDF-twin status.
- **Simulation**, 4 turns: save, append, and read back; search to the Conclusions line; overwrite backup then `edit_file` confirmed by `read_file`; `read_document`.
- **Meta**: the inventory question exposed the cache issue above and, after the fix, listed all six analysis tools; a delegation to analysis used `read_document`, `examine_data`, `save_file` and `read_file` and returned the file's content.
- **Adversarial sweep** of every tool in every mode (26 cases each; plan has no `read_document` by design): traversal on save, append, edit and rename; empty content and files; a directory as a file; `max_lines=0`; unicode and CRLF; binary through read and edit; malformed JSON; bad regex; offset beyond EOF and negative; tail larger than the file; ambiguous snippet, `replace_all`, batched atomicity on a miss; four-deep overwrite chain with distinct backups; rename onto an existing file and copy; mixed `read_document` with unsupported and missing entries; a 20 000-key JSON windowed and searchable; a CSV with quoted commas and empty cells. All pass.

### Not in scope

The meta's `view_document` (#397's remaining phase) and the meta inventory's description truncation, which hides `read_file`'s parameter list from the meta's routing text (the specialist itself sees the full schema).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4993945c524f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.