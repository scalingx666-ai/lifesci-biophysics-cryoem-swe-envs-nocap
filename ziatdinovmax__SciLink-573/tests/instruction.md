# Issue

**Title:** Web live inset: stream best-of-N candidate figures as they appear

## Description

.

## Summary

The "watch the analysis" inset only lit up when a promoted winner appeared, which for a single image or spectrum with best-of-N is the very end of the run. Candidate figures now stream as each candidate writes its canonical figure, labeled "candidate N", and the promoted winner supersedes them when the item completes. The rest of the candidates' internals stays hidden.

## Technical description

`scilink/server/runner.py`: `_IMAGE_SKIP` still lists `_candidates`; a narrow exception (`_CANDIDATE_VIZ`, `/_candidates/cand_NN/visualization.png`) lets the canonical figure through and `_image_label` names it `candidate N`. The scanner is shared by both foundation agents that use the `_candidates` + promotion layout (`_locked_exec.py`), and the per-tick emit cap still applies. Test extended in `tests/test_web_server.py::test_scan_new_images`: the candidate's `visualization.png` is emitted with the candidate label, its `debug_masks.png` is not.

### Live checks (Bedrock, Opus 4.8, meta sessions)

Single image, best-of-3: the inset received candidate 0, candidate 2 and candidate 1 during the escalation, then the promoted `visualization`. Single spectrum, best-of-3 (curve fitting): candidates 0/1/2 streamed through the escalation and each refit, then the promoted figure. Hyperspectral: the dashboards streamed as before.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.