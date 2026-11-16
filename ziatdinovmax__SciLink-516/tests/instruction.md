# Issue

**Title:** Sequential uploads stay independent; the orchestrator judges series-ness

## Description

## Summary

Uploading a file, analyzing it, then adding a second file later silently turned both into a "series": Streamlit's uploader accumulates files across reruns, and the UI branched on the count alone, merging everything into `uploads/series/`. Per the re-scoping discussion on the issue, the fix moves the decision to where it belongs: the UI now only records the one signal it uniquely has — which files arrived together and which arrived later — and the orchestrator decides what actually belongs together, asking the user only when it's genuinely ambiguous. No new widgets or toggles; files added later are saved as independent uploads, and a genuinely dragged-together batch keeps today's series mechanics with the agent asked to *confirm* series-ness rather than having it asserted.

.

---

**Technical details**

- `split_upload_arrivals` (sidebar.py): partitions the accumulating widget value into first-batch vs late additions — first non-empty rerun defines the batch; stable across reruns; resets when the uploader is cleared; state injectable for tests.
- Both upload surfaces rewired (pre-chat zone in `chat_uploads.py`, sidebar uploader): late additions go through `save_upload` individually (never `save_upload_batch`), so no `series/` merge and no re-pointing of `uploaded_data_path` at a directory.
- `build_analyze_prompt` (extracted, pure): late-arrival uploads are listed with their arrival groups and flagged "likely independent — decide what belongs together, ask if genuinely ambiguous, analyze independent files separately"; together-batches get "confirm they actually form one series" (composing with the metadata/sidecar variants). Single-file prompts are byte-identical to before.
- `save_upload` / `save_upload_batch` now return the saved path (previously returned None).

**Verification**

- Offline (`tests/test_upload_arrival_intent.py`, 8): partition semantics (stability, late detection, clear-reset), prompt shapes, and the #114 repro against the real save helpers — a late file lands flat with no `series/` created; a true batch still becomes `series/`. UI regression suites pass (56 total).
- Live (Bedrock opus-4-8), three scenarios asserted from the session's `events.jsonl` (#513): a late-arrival unrelated pair → "two independent datasets, analyzed separately", one successful per-file `run_analysis` each, no directory run; an unrelated pair sitting in `series/` under the batch prompt → "two files that do not form a series", analyzed per-file; and the control — a genuine 3-file temperature series under the same prompt → confirmed and run as ONE series analysis on the directory with proper `series_metadata` (no over-correction). In two runs the #411 metadata-ownership guard fired mid-scenario and the agent recovered with a clean retry — correct composition of both features.
- Known limitation: the literal Streamlit widget wiring is covered by simulated-rerun tests calling the real save helpers; a browser-driven test wasn't possible in this session (extension unavailable). The accumulate-across-reruns widget behavior is the documented Streamlit semantics the issue itself describes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `289a874f94ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.