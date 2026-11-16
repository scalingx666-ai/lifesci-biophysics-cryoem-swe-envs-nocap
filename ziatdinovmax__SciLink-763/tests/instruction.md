# Issue

**Title:** Engine-neutral parameter convergence for plane-wave DFT (VASP ENCUT + k-points)

## Description

# Summary

Adds an **opt-in, engine-neutral parameter-convergence sweep** for plane-wave DFT: step a numerical parameter up a declared ladder until its observable plateaus within a tolerance, then adopt the cheapest *trustworthy* setting before the production run.

Today SciLink chooses the plane-wave cutoff and k-points with a single LLM call and no numeric convergence check — so a computed lattice constant or adsorption energy rests on *chosen, not tested* settings. This makes the choice measured, and never lets the sweep drop a setting below the one input validation already approved.

## Design (follows existing patterns, nothing engine-specific in shared code)

**Engine-neutral core** — `scilink/agents/sim_agents/convergence.py`:
- `converged_setting(observations, tol)` — the comparator: the cheapest setting past which the observable stays within `tol` of the most-accurate rung; reports `not_converged` if still drifting at the top rung.
- `run_convergence_sweep(...)` — batch fan-out over a declared ladder, with `build` / `run` / `read` callbacks injected (no engine knowledge here).
- `converge_parameters(...)` — sequential, adopt-as-you-go across several parameters, with a **floor**: each ladder is trimmed at the base deck's current value (via a `get_convergence_param` hook + a per-spec `direction`), so a plateau the observable happens to reach at a lower rung can't pull the adopted setting below the validated minimum (e.g. VASP's ENCUT ≥ 1.3× ENMAX). A parameter whose base value can't be read safely (`skip_without_floor`) is skipped rather than swept blind.

**Skill-declared specs** — the *engine skill* says what to converge via a `convergence:` frontmatter block (ladder, observable, tolerance, direction), consumed the same engine-neutral way as existing skill frontmatter:
- **VASP** (`periodic_dft/vasp`): ENCUT and k-points (expressed as KSPACING, reported back as an `n×n×n` mesh); observable = energy/atom. Decks are read/written with pymatgen (`Incar` / `Kpoints`), so a tag anywhere on a line is handled and the k-point floor works whether the deck carries INCAR `KSPACING` or an explicit `KPOINTS` mesh.

Each skill provides `set_convergence_param` / `get_convergence_param` / `read_convergence_observable` hooks, resolved through the skill registry exactly like `default_run_command`.

**Pipeline** — a new opt-in `converge_parameters` step (off by default), mirroring the existing `reference_check` / `potential_selection` step family: when enabled for `periodic_dft` with an executor, it resolves the skill's hooks, runs the sweep, adopts the converged inputs (and clears each step's run folder so a retry can't read a previous attempt's output).

## Validation

- **80+ unit tests**: comparator semantics on synthetic and real observable ladders (including a real VASP Cu energy-vs-KSPACING regression fixture), the ladder floor, VASP hook behavior (pymatgen read/write, KPOINTS→KSPACING derivation, registry resolution), and the opt-in pipeline step driven by a mock executor.
- **Live value-accuracy, real VASP**: a Si case run end-to-end — converge ENCUT + k-points, adopt, relax, and score the relaxed lattice constant against experiment. The floors pinned ENCUT/KSPACING at the validated base values live, the sweep converged, and the relaxed **a = 5.469 Å vs 5.431 Å (0.69 %, well inside the 2.5 % band)** — i.e. the converged settings produced a physically correct value, not just a plateau.

## Scope

- **Off by default** — purely additive; existing runs are unchanged unless `converge_parameters=True`.
- **No QC basis-convergence sweep.** A plateau-style sweep fits a knob whose value flattens as it increases (plane-wave ENCUT / k-points). A Gaussian basis sweep on the *absolute* energy does not flatten — it approaches the CBS limit only slowly (~L⁻³), so "adopt the smallest basis past which the energy stops changing" has no reachable plateau. QC basis adequacy belongs to a property / energy-difference target or a CBS extrapolation (a different mechanism), so this PR declares no QC convergence sweep. The NWChem output reader (`read_convergence_observable`, with correlated-energy parsing validated against real TCE + classic-module logs) stays as a reusable hook.
- MD/transport observable convergence is a **separate** concern (tracked elsewhere) and intentionally not in this PR.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47a3de5f8a5c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.