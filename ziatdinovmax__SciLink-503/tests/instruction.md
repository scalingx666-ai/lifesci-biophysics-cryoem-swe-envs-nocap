# Issue

**Title:** Anneal the whole skill with temperature at every stage

## Description

.

## Summary

When an analysis needs a second, "hotter" attempt, the agent is supposed to relax the domain skill it was given — so that if the skill's usual model is wrong for this data, the re-run can leave it behind. Until now that relaxation only happened inside the fitting loop: the **planning** stage still treated the skill as mandatory at every temperature, so a hot re-run re-derived the same skill-bound plan and could only deviate while patching the fit. Interpretation likewise always spoke as if the skill were mandatory, even after the fit had abandoned it.

Now one temperature governs the skill everywhere — planning, plan validation, code generation, conformance checking and interpretation — moving uniformly from *mandatory* (first run) → *guidance* → *reference* (hot re-run). A first run is unchanged: the wording the model sees at the default temperature is byte-identical to before, for all three analysis agents.

**Live check (Opus 4.8):** the `xps` skill applied to a single-line Raman spectrum. Every prompt at every stage carried the expected frame (`MANDATORY Domain Skill Rules` on the first run, `Domain Skill Reference` on the hot re-run); both runs recovered the true line position and width (1332.40 cm⁻¹, FWHM 3.2, R² = 0.999). One live finding shaped the change: with no concrete plan present, the skill's imperative code recipe ("always follow this exact sequence") dragged a hot run back to a Shirley background + spin-orbit doublet on a Raman line even when offered only as "reference" — so relaxed levels now state explicitly that the locked plan wins where a recipe conflicts with it, and the conformance check ignores the skill entirely at the hot level.

---

## Technical details

- **Per-controller `_SKILL_STRICTNESS_FRAMES` + `_skill_annealing_level(state)`** (curve, image, hyperspectral). The resolver reads the live `_annealing_level`, falling back to `_starting_annealing_level` — planning runs before the QC engine seeds the level, so a re-run launched hot re-plans relaxed. Frame 0 is the previous hardcoded wording verbatim (verified byte-identical against `main` for every stage and state shape).
- **`_append_skill_context`** (planning / validation / interpretation) frames by temperature; **conformance** framing anneals (curve already used the schedule; image's hardcoded `**Domain Expertise**` now uses the frame label); codegen/correction preamble reads go through the same resolver (also covers the locked-script reuse path, which ran before the engine seeded the level).
- **Best-of-N winner lock** (`_record_winner_annealing_level`, curve + image): candidates run in shallow state copies, so the engine's level sync never reached the caller's state — interpretation would have read T=0 after a hot fit. The reached level is stamped at winner lock (monotone).
- **Hyperspectral**: no state-level temperature (its `analyze()` has no `starting_annealing_level`), so planning/interpretation resolve to T=0 there; but the codegen retry ladder had the same collision. The base prompt is built by a per-level function (`ctx.base_prompt_builder`) and `qc_refine` re-renders it at the attempt's rung.
- **T≥1 codegen preambles** (curve + image): "Where these rules conflict with the locked plan, follow the plan." **Conformance at hot** drops the skill rules; the checker's only job is plan fidelity. T=0 text untouched. Not ablated live — the clean runs succeeded with this in place; happy to run the ablation if wanted.
- Physics grounding stays the baseline's job (best-of-N plausibility rubric + residual diagnostics); no non-anneable skill carve-out.

**Tests:** `tests/test_skill_annealing_all_stages.py` (39): frozen wording pinned as literals, relaxation at T=1/T=2, hot re-run re-plans, `_annealing_level` precedence + clamping, conformance prompts captured per level, winner propagation, hyperspectral ladder re-render. Related offline subset: 551 passed; the failures in `test_hs_dynamic_records::test_retry_ladder_reaches_hot_and_succeeds` and `test_curve_fitting_orient_and_adapt` are pre-existing on `main`.

**Known nit (pre-existing, not addressed):** `_stamp_hot_deviation` keys on `_produced_at_level`, which only refits carry, so a hot-*start* initial attempt gets no `deviation_note`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `30f83c7ad67c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.