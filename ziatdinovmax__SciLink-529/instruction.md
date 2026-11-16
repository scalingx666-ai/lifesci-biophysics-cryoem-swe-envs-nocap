# Issue

**Title:** Refresh figures rewritten in place (chat figure + inset)

## Description

## Summary

Reported live: in a running session, a figure the agent rewrites in place — e.g. a refined `visualization.png` at the same path across turns — kept showing the **first** render. Neither the figure attached to the chat nor the floating analysis inset updated. This was two independent bugs stacked on the same path; both are fixed.

**Why it happened**

1. **The completion sweep de-duplicated images by path alone.** `ArtifactTracker` keyed its "already surfaced" set on the bare file path, while the HTML-report and Markdown sweeps have always keyed on `path + mtime`. So a rewritten `visualization.png` at a known path was judged already-seen and never re-attached to a later assistant message.

2. **The image URL had no version token.** Even though the live inset watcher *is* mtime-aware and re-emits on rewrite, the `<img src>` (`…/files?path=visualization.png`) was byte-identical across rewrites, so the browser served cached bytes — and the frontend reducer additionally dropped the re-emit as a same-path duplicate.

Both had to be fixed for the figure and the inset to update.

## What changed

**Backend**
- `artifacts.py`: image identity is now `path + mtime_ns` (new `_img_key`), applied in `find_new_images`, `mark_all_existing`, `mark_known` — a rewritten figure re-surfaces at turn completion; an unchanged one still doesn't.
- `runner.py`: `_scan_new_images` now carries the file `mtime_ns` and threads it into each `analysis_image` event as a version token `v`.
- `app.py`: `get_file` sends `Cache-Control: no-cache` (revalidate — `FileResponse`'s ETag keeps an unchanged file a cheap 304). `get_thumb` already did.

**Frontend**
- `api.fileUrl(id, path, v?)` appends `&v=<mtime>` (backend ignores it); `AnalysisInset` uses it and a version-keyed `<img>`.
- `useSessionEvents` / `App.tsx`: the `analysis_image` event carries `v`; the inset reducer de-dups on `path + v`, so an in-place rewrite (same path, newer version) refreshes while an identical reconnect replay is still suppressed.

## Tests
- New `tests/test_artifact_image_rewrite.py`: a rewrite re-surfaces; resume (`mark_all_existing`) marks only the current version and lets a later rewrite through; an unchanged file never repeats.
- `tests/test_web_server.py`: updated for the `_scan_new_images` 4-tuple/version token, plus a rewrite-re-emits assertion.
- Full `test_web_server.py` + new test green (31 passed); `webui` `tsc -b && vite build` clean.

Branches off `feature-react-webui` (the web server lives only there, not on `main`).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e41b37802f2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.