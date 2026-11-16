# Issue

**Title:** Publish every ~/.scilink store write atomically (part 1 of #398)

## Description

## Summary

The `~/.scilink` store (graduated skills, script bank, knowledge-base manifests, config files) is shared by every SciLink session on a machine, but its files were written in place — a session reading a file while another session was mid-write could see a half-written file. Worst case, a half-written script-bank record is skipped by readers forever, so a proven script silently disappears from retrieval with no error anywhere. Every store write now goes through a temp file published by an atomic rename: a reader always sees either the old file or the new file, whole — never a torn one.

This is part 1 of #398 (torn-read elimination). Part 2 — locks around the read-modify-write hot spots (lost stat increments, duplicate records, concurrent skill-graduation merges) — remains on the issue and involves a dependency decision (`filelock`).

---

**Technical details**

- New `atomic_write_text(path, text)` in `scilink/utils/text_io.py`: same-directory `mkstemp` + `os.replace` (the pattern `_tool_cache.py` already used), UTF-8 pinned like the module's other helpers, temp file unlinked on failure. It preserves an existing destination's permissions and honors the umask for new files, so rewritten store files keep their readability (plain `mkstemp` would have silently tightened `0644` → `0600`); confirmed byte-identical content vs the old writes.
- All 21 store write sites routed through it: script-bank records (5 sites — including the three added by the edit-adapt work after #398 was filed), staging (4), skill graduation, `scilink memory` markdown writes (5), the KB manifest, `config.json`, `wizard_state.json`, `available_software.yaml`. A sweep confirms no plain `write_text` remains in any store module. `_reconcile.py`'s HTML write targets a session artifact, not the store, and is untouched; the tool cache's existing atomic publish is untouched.
- Existing stored files are unaffected until their next update, which produces byte-identical content under a new inode; no migration, no read-path change.
- Tests (`tests/test_store_atomic_writes.py`, 6): overwrite + no temp droppings, permission preservation + umask, unicode round-trip, failure cleanup, a 4-process hammer where a continuously-reading parent must never observe a torn file, and concurrent script-bank `add_record` leaving every record parseable (the masked-data-loss mode). Existing suites for all touched modules pass: 213 tests across script bank, bank edit-adapt, KB store, persistent memory, graduation, loader, and memory panel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bf34534b9ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.