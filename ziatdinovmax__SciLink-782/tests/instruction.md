# Issue

**Title:** sim: engine-neutral health gate — catch a run that completes non-physical

## Description

Closes the correctness gap in #781.

## Problem

In any simulation, a run can finish "successfully" in a physically nonsensical state and never be caught. The failure mode that motivated this (MD): a barostat drives the box to a near-vacuum — equilibrated density orders of magnitude too low — but the engine exits 0 (a dilute gas is a valid state, no crash), so the run is handed downstream as a normal completed run.

The autonomy's self-correction (method escalation, reparameterization) only fires on a **converged observable compared to a reference** — a run that lands in a non-physical state never reaches that comparison, and there's no tripwire ahead of it.

## Fix (engine-agnostic — mirrors the `convergence:` pattern)

Skill declares a spec in frontmatter; a per-engine hook reads the value; the driver stays engine-neutral.

- **`health.py`** — pure, I/O-free comparator. `evaluate_health(observations, specs)` returns the declared bands a run violated. An unreadable (`None`) value is skipped, not failed; a non-finite value always fails. The bands are **gross physical-sanity bounds, not accuracy checks**.
- **`refinement.py`** — the phase runners (`_refine_phase`, `_run_once_phase`) gate each finished run. On an out-of-band observable the run is re-run (optionally perturbing initial conditions via an engine `perturb_for_retry` hook) up to `max_health_retries`, then marked failed with `failure_class="health_gate"`. Without a perturb hook, re-running an identical deck can't help, so the gate fails fast. The perturbation touches only initial conditions, never the deck — so a replica ensemble's identical-deck invariant is preserved.
- **LAMMPS** — `read_health_observable` reads a gross-sanity observable (density/temperature/pressure/volume/energy) from the thermo log, and a `health:` block in `lammps.md` declares a loose density band. MD was the one engine with no health spec; an engine that declares none stays ungated.

## Validation against real engine output

Beyond unit tests, the LAMMPS reader is grounded in **real `log.lammps` fixtures** (trimmed to the final thermo block plus real end-of-file noise — a truncated row, an FFT-grid line, a LAMMPS ERROR line), so the parser is tested against genuine engine bytes, not a reconstruction of the format:

| fixture | real reader result | gate |
|---|---|---|
| healthy completed run | density **0.831** | **PASS** |
| barostat-blown box | density **0.00257** | **FAIL** (`< min 0.02`) |
| stress-tensor deck (no density col) | **None** | **SKIP** |

27 health tests; existing refinement/pipeline suites green.

## Scope

Minimal gap-closer. Deferred fast-follows:
- **Deck-aware observables** — the deck's `thermo_style` declares what's logged (readable via the existing `detect_signal_logging`), so the gate should check the health observables the deck *actually logs* (and/or require it to log at least one), rather than silently skipping. This supersedes a fixed density-only check and handles e.g. stress-only viscosity decks.
- `perturb_for_retry` (engine reseed → retry self-heals seed-dependent pathologies).
- `snapshot_run` for MD (structured final state to the critic instead of a log tail).
- Per-member throughput watchdog; decouple the campaign so one stalled member doesn't block analysis of the rest.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b2d013c72143`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.