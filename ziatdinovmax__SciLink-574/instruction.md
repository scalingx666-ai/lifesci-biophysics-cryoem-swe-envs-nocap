# Issue

**Title:** Worker-agents panel: image and hyperspectral agents persist state from the start

## Description

.

## Summary

The Worker-agents panel stayed blank for image-analysis delegations. Both the image and hyperspectral agents now record an `analysis_started` action the moment a run begins, so they appear in the panel while running and after, with friendly names ("Image Analysis", "Hyperspectral Analysis").

## Technical description

The panel reads `<agent_type>_state.json` and lists a worker once `action_history` is non-empty. The image agent only logged one action at the very end of `analyze()` (nothing on the early error path), and never initialized its state with `agent_type`; hyperspectral logged at the end / on error. Now: `ImageAnalysisAgent.analyze` calls `_init_state` + `_log_action("analysis_started", ...)` after input normalization and logs the failed-load path; `HyperspectralAnalysisAgent.analyze` logs `analysis_started` after its `_init_state`. `telemetry._AGENT_LABELS` and `delegations._sub_agents` get the two labels. Test: worker rows for an in-progress image run and a completed hyperspectral run (`tests/test_meta_telemetry_reader.py`).

### Live checks (Bedrock, Opus 4.8, meta sessions)

Image delegation: "Image Analysis" appeared in the Worker-agents data with `analysis_started` while the delegation was still running (about 30 s in), then completed with the `image_analysis` action. Hyperspectral delegation: "Hyperspectral Analysis" appeared with `analysis_started` within two minutes and finished with `analyze`; `hyperspectral_state.json` on disk. Curve fitting still logs only its final action (not in this issue's scope).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cbf8e2980900`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.