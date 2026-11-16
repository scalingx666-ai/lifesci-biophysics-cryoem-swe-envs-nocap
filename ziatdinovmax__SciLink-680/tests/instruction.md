# Issue

**Title:** Toward #447: post-run observable-convergence detection

## Description

## Summary

Toward #447. Adds **detection** of non-converged post-run observables, so the pipeline stops silently reporting a value nobody should trust. It does **not** yet auto-close the loop — see "What this does not do."

- New orchestrator tool **`check_observable_convergence`**: runs the simulation analysis agent on a finished run directory and reports, per property, whether it converged — with the convergence flag, value, and verification reasoning as evidence.
- **Diagnostic-only by design.** It reports the finding, never a remedy. Non-convergence has many causes (equilibration, state point, force field, sampling cadence, finite size), so steering the caller toward one fix ("extend / add replicas") can point at the wrong one — in live testing a replica's implausible value came from a too-coarse sampling *cadence*, where "add more replicas" would have reproduced the defect. The caller diagnoses from the evidence instead.
- System-prompt workflow step so the orchestrator runs the check after MD production runs.
- Drive-by bug fix: cap the analysis-script filename length (`base_analysis_agent`), which raised `ENAMETOOLONG` after a successful run when the research goal was long — losing the computed result. This also affects `analyze_output`, independent of the new tool.

DFT/single-point runs are unaffected: convergence flags are only emitted by time-series analysis skills, so those workflows report `converged: true` and pass through unchanged.

## Live testing (Deception HPC, TIP3P water, real LAMMPS)

- The orchestrator called `check_observable_convergence` **unprompted** after the production run, and correctly flagged the result as not converged.
- On a `converged: false` verdict it launched longer replicas on its own.
- The tool correctly flagged **every** unconverged result, including physically impossible ones (a negative viscosity from an under-sampled replica).

## What this does not do (follow-ups)

Closing the loop needs more than detection, and the live run surfaced why:

1. **Replica consistency.** The orchestrator adds replicas as independent `run_simulation` calls, each of which regenerates the deck from scratch, so replicas drifted in production length and stress-sampling cadence and can't be pooled. Robust fix = reuse the settled deck varying only the seed (a deck-reuse path over the existing sweep expander), not prompting.
2. **Sampling cadence.** The deck generator can log the pressure tensor too coarsely for Green-Kubo. The `lammps` skill already says "every few fs", but the machinery that enforces it (`ObservableRequirementsDeriver` -> planner/deck-gen cadence requirement) is dormant because `derive_observables` defaults off.
3. **Pooled multi-replica analysis** and a repeatable (locked) analysis script.

I'll open a separate issue tracking these so the convergence loop can actually close; this PR is the detection half.

## Tests

- New `tests/test_check_convergence_tool.py` (13): flag-scanning logic + tool integration with a monkeypatched analysis agent; asserts the tool is diagnostic-only (no `recommendation` field).
- New `tests/test_convergence_live.py`: opt-in live scenarios (proactive detection; the real-LAMMPS loop).
- `test_sim_base_analysis_agent.py`: filename-cap test.
- `test_simulation_orchestrator.py`: `check_observable_convergence` added to `EXPECTED_TOOLS`, plus `get_history_events`/`search_session_history` that upstream registers but never listed (so `test_1_orchestrator_constructs` was already failing on main). Targeted tier now 14/14.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b988c7cdab5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.