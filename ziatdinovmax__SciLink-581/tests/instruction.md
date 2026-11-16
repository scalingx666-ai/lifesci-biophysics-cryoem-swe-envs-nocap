# Issue

**Title:** Probe the script-execution environment; prescribe only what it can run

## Description

Re-opened from #578, which GitHub auto-closed when its stacked base branch (#577, now merged) was deleted. Same single commit, now one commit ahead of main.

.

## Summary

The planner and verifier could prescribe, even hard-mandate, a fix that needs an optional capability the script-execution environment does not have — SAM was one instance. The image agent now probes that environment once (packages importable in the subprocess interpreter, CUDA/MPS, SAM checkpoints on disk) and shows the result to the planner, code generator, verifier and refiner with one rule: only prescribe what is listed available, keep a fallback for optional capabilities, never hard-mandate an absent one. The SAM tool reports structured unavailability instead of throwing.

## Technical description

- `scilink/agents/exp_agents/_exec_env.py` (new): `probe_execution_environment(executor)` runs `PROBE_SCRIPT` through the `ScriptExecutor` (the `sys.executable` subprocess — the load-bearing subtlety: it can differ from the process that imported scilink), cached per interpreter; a failed probe degrades to `status: "unknown"` (treat every optional capability as unverified). `exec_env_block(env, for_verifier)` renders present/absent packages, compute device, checkpoints and the rule; `capability_available(env, name)` is the programmatic check.
- `image_analysis_controllers.py`: the block is appended to the planning prompt (after the tool inventory), the codegen context, the verifier prompt and the refinement prompt.
- `sam.py`: `run_sam_analysis` returns `{status: "unavailable", capability, reason, empty results}` on ImportError / missing checkpoint / load failure, and the `TOOL_SPEC.returns` text says so.
- Complements #568 (a stalled prescription pivots) and #567 (SAM prefers MPS once installed). Curve-fitting and hyperspectral can adopt the same block; the helper is modality-neutral.
- Tests (`tests/test_exec_env.py`, 5): the probe runs through the executor and is cached; failure → unknown; block content and the verifier-only sentence; the SAM tool's unavailable dict; planner, refinement and verifier prompts carry the block.

### Live checks (Bedrock, Opus 4.8)

With `segment_anything` absent from the execution interpreter, the probe reported it missing (`Execution environment probed: … missing: hyperspy, segment_anything; devices cuda=False mps=True`); the image delegation planned and verified on watershed and never prescribed SAM. In the induced-stall harness (#577) the stubbed verifier prescribed SAM anyway and the block reached the refiner and verifier prompts.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9f1b8636e71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.