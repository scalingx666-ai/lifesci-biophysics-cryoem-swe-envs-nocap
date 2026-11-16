# Issue

**Title:** Image verification: pivot off a stalled prescription instead of repeating it

## Description

.

## Summary

When the verifier kept prescribing a fix the code generator could not make take, the run repeated it until the verification budget was gone. The image verification loop now recognizes a stalled prescription — the same fix issued three iterations in a row with no score improvement — names it to both the refiner and the verifier as "do not re-issue: choose a materially different path or accept the best result", and if it comes back once more anyway, stops with the best result so far and a legible reason recorded on the result.

## Technical description

- `image_analysis_controllers.py`: `_same_prescription` (token Jaccard ≥ 0.5, so rewordings count; "none" never matches), `_detect_stalled_prescription` (consecutive tail of `verification_history` with the same action, `STALL_TIMES = 3`, best score since first appearance within 0.02), `_note_stalled_prescription` (records in `state["_stalled_prescriptions"]`, warns; a repeat after the pivot sets `ctx.stop_reason`), `_stalled_prescriptions_block` (added to the refinement prompt via a new `stalled=` parameter of `_apply_verification_feedback` and to the verifier prompt in `_verify_quality`), `_stamp_stalled` (result field `stalled_prescriptions`, applied in `qc_post_verification`).
- `_qc_engine.py`: `QCItemContext.stop_reason`; after each refinement the loop logs it and takes the final-verify path exactly as for the wall-clock budget.
- Tests (`tests/test_stalled_prescription.py`, 5): rewording tolerance; the detection rule; pivot then stop with the result stamp; the prompt block; the engine stopping on a host's reason (minimal fake host). The two `qc_golden` image failures are pre-existing on main.

### Live check (Bedrock, Opus 4.8 — induced stall)

Real image agent end to end (planning, code generation, sandbox, refinement) with the verifier replaced by a stub that always scores 0.5 and prescribes "invoke run_sam_analysis and substitute its masks" while `segment_anything` is absent from the execution interpreter. Budget 7 iterations. Observed: verifications 1–3 repeat the fix; after the third the host logs "Prescribed fix did not take after 3 tries (score 0.50 → 0.50) … pivoting"; the refiner prompt carries the stalled block from call 3 and the verifier from call 4; verification 4 re-issues it and the loop stops ("Stopping verification: prescribed fix … did not take after 4 tries; keeping the best result so far") — 4 of 7 iterations used. The returned result carries `stalled_prescriptions` (times 4, score 0.50 → 0.50); the first two passes exposed that the stamp reached the persisted item but not the returned dict, fixed in two follow-up commits. Run time about nine minutes per pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.