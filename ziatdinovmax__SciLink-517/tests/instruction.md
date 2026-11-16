# Issue

**Title:** Scale-neutral simulation recommendations

## Description

## Summary

After analyzing experimental data, SciLink could recommend theory follow-ups — but only DFT ones: the recommendation agent's prompt, schema, and tool were all DFT-specific, so an analysis whose natural next step is molecular dynamics (coarsening, diffusion, melting) or molecular quantum chemistry still came back with DFT suggestions. Recommendations are now *simulation studies*: a structure to model, a research goal saying what to compute and which experimental question it answers, and an optional scale suggestion. The suggestion is deliberately non-binding — the analysis side recommends the **what**, and the simulation router keeps ownership of the **how** (final scale and engine), the same contract philosophy as `DeployedPotential`.

. (Per the re-scoping note on the issue: items 2–3 of the original plan targeted a `workflows/` layer that no longer exists; this delivers the re-scoped item 1 + tool surface.)

---

**Technical details**

- `RecommendationAgent.generate_simulation_recommendations_from_text` with new prompt (`TEXT_ONLY_SIMULATION_RECOMMENDATION_INSTRUCTIONS`); per-recommendation schema adds `research_goal` (required in the prompt, optional in validation) and `suggested_scale`. The scale vocabulary is exactly the simulate router's: `periodic_dft / molecular_qc / molecular_dynamics` — notably `machine_learning_potentials` is NOT offered (an MLIP is a potential *source* chosen downstream of routing, #429); an invalid hint is dropped rather than passed on. `VALID_SCALES` is a mirrored literal (exp_agents must import without the optional sim stack) with a drift-guard test pinning it to `simulation_router.DEFAULT_SCALE_DESCRIPTIONS`.
- Tool surface: registered as `recommend_simulations` (analysis system-prompt guidance updated: MD-scale recommendations are goals to hand to simulate mode; DFT-scale ones still feed `run_dft_workflow` by `recommendation_index`). Back-compat throughout: `recommend_dft_structures` still dispatches via `functions_map` (not in the LLM schema), `generate_dft_recommendations_from_text` aliases the new method, recommendations persist under both `simulation_recommendations` and the legacy `dft_recommendations` record keys, and `run_dft_workflow` reads either.
- Sidecar moves to `simulation_recommendations/simulation_recommendations.json` under the analysis record's directory.

**Verification**

- Offline: 6 new tests (`tests/test_simulation_recommendations.py`) — validation incl. the MLIP-hint drop, back-compat method alias, schema/functions_map split, dual-key storage + sidecar, system-prompt consistency, and the router drift guard; the refine-interpretation consumer invariant (`_effective_full_result` routing) updated to the new name and passing.
- Live (Bedrock opus-4-8), 14/14: a DFT-shaped STEM dopant analysis → orchestrator picked `recommend_simulations` unprompted, 8 studies with substantive goals, 7/8 hinted `periodic_dft`; a dynamics-shaped in-situ coarsening analysis → 6/8 hinted `molecular_dynamics` with MD-shaped goals (heating-ramp premelting onset, coalescence dynamics) and sensible structure downscaling; and the router contract both ways — with engines declared for all three scales, the real simulate router routed the MD-scale recommendation to `molecular_dynamics`/lammps and the DFT-scale one to `periodic_dft`/vasp. Environmental note: on an engine-less machine the router correctly reports "no engines available"; the test declares fixture engines via `SCILINK_AVAILABLE_SOFTWARE`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `289a874f94ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.