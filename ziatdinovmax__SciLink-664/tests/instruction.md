# Issue

**Title:** Port simulation analysis to CodegenQCEngine (#439 Tier 2, part 1)

## Description

## Summary

- Wires the shared `CodegenQCEngine` into the sim-side `BaseAnalysisAgent`, following the hyperspectral adoption pattern (`config_key=None`, `refine_anchor="none"`)
- Adds **verification-quality retries** with progressive constraint annealing: when a script runs successfully but the result is judged implausible, the engine regenerates with feedback and escalating freedom (constrained → relaxed → hot). The technique recipe stays in the prompt at every level (it is what tells codegen how to read the engine's output); at hot it becomes advisory rather than binding.
- Preserves the existing **crash-recovery loop** inside `qc_run_initial` for the initial attempt. A refit that crashes ends the verification loop (the engine's `refit_fail_msg` path) and the best result so far is returned — this is asymmetric with the initial attempt, deliberately.
- Two knobs: `max_refinement_attempts` bounds crash retries on the initial attempt; a new `max_verification_iterations` (default: same value) bounds verification refits. Each verification iteration is one generation + execution + verification, plus one final verification after the last.
- The `compute_property` public API and return format are **unchanged**. Every returned result carries the verification that judged *that* result.

Part of #439 — this is the engine port, the first half of Tier 2.

## What this adds over the previous `compute_property` loop

| Capability | Before | After |
|---|---|---|
| Crash recovery (script errors) | ✅ retry up to `max_refinement_attempts` | ✅ same for the initial attempt; a refit crash ends the loop |
| Verification-quality retries | ❌ implausible results returned as-is | ✅ retry with annealing feedback, up to `max_verification_iterations` |
| Constraint annealing | ❌ | ✅ 3-level schedule (constrained → relaxed → hot, recipe kept as advisory at hot) |
| Script bank / locked reuse | ❌ | ⏳ `qc_try_reuse` is a stub returning `None`; nothing sets `ctx.reuse_script` yet |

## Next (rest of Tier 2, follow-up)

- A `"simulation"` domain for `_script_bank` plus an output-shape fingerprint
- `qc_try_reuse` reading a real `reuse_script` and running the locked script
- `try_bank_edit_adapt` / `record_bank_assist` / `bump_bank_adapt_success` from the hooks, with a sim-flavoured output-contract text for `BANK_EDIT_ADAPT_INSTRUCTIONS`
- Decide whether one `compute_property` call (one property = one script) is the unit that gets banked

## Test plan

- [x] All pre-existing `test_sim_base_analysis_agent.py` tests pass (API compatibility)
- [x] All `test_simulation_analysis_agent.py` and `test_synthesis_reentry.py` tests pass
- [x] `TestQCEngineIntegration`: verification retry, bypass, crash recovery, annealing feedback, fallback-on-exhaustion, **plus** (from review): returned verdict judged the returned value (distinct values per attempt), final-verify verdict kept on reject, refit score left unverified, recipe kept as advisory at hot, separate verification knob
- [ ] Manual smoke test with real simulation output (trajectory → property computation)

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01XYvkT6x9VFTBCyY33DMLPL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b8e2d137d8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.