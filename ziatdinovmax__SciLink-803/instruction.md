# Issue

**Title:** Refinement: never accept a run whose engine failed

## Description

.

## Summary

The refinement loop accepted a phase on the post-run critic's verdict alone, so a crashed run the critic called a "warning" was reported as a success. In the run behind #802, NWChem exited 255 after TDDFT refused benzene's non-Abelian symmetry. No excited states were computed, yet the phase and the workflow reported `success`.

The executor's own facts now gate acceptance, the way the health gate already holds a measured non-physical run:

```
engine failed (status "error", or returncode != 0)  +  critic says good/warning
        →  verdict held at needs_fixes (critic's fixes kept)  →  repair loop, or the phase fails with the reason
```

## Changes (`refinement.py`, `simulation_pipeline.py`)

- **`_engine_failure(result)`:** a launch error, timeout or stop (`status == "error"`), or an int `returncode != 0`. An unknown exit code (`None`, e.g. a scheduler that reports none) keeps trusting the critic.
- **`_hold_failed_run`:** applied in `_refine_phase` (after the health-gate override, before the policy decision and `ctx.record`) and in `_run_once_phase` (combine steps). It covers sequential steps, fan-out members and resumed members, which all go through `_refine_phase`.
- **The reason reaches the model.** It's on the phase record (`engine_failure`, set whenever the engine failed, whatever the critic's verdict), the campaign result (`engine_failures`) and the workflow's `warnings`. `run_simulation`, `run_complete_dft_workflow` and `run_dft_workflow` return those `warnings` to the model, which also carries the health-gate and force-field caveats.
- **Held runs are logged:** `phase <name> held: <failure> (the critic said <verdict>)`.
- **The dry-run gate is unchanged.** Its trimmed twin can exit non-zero on a deck that runs fine (e.g. a LAMMPS deck reading a file it writes earlier), and the gate is fail-open by design.

I checked the engines' run commands for any where a non-zero exit is normal (VASP via `mpirun` plus the POTCAR pre-run `&&`, LAMMPS, NWChem, ORCA, the ASE/MLIP driver, the HPC batch wrapper's `echo $?`). There is none.

## Testing

- **`tests/test_refine_engine_failure.py` (new, 18 tests):**
  - crash + "warning" is not a success;
  - a crash the critic can fix is refined and accepted;
  - timeout, signal kill, and a crashed combine step;
  - an unknown exit code and a clean "warning" still pass;
  - a "poor" verdict keeps its word but still names the cause;
  - a held run is logged, and a clean run isn't;
  - a crash followed by a clean re-run leaves no cause on the record;
  - a met fan-out quorum names the crashed member without failing;
  - all three tools return the workflow's `warnings` (plus one test in `test_simulation_recommendations.py`);
  - Autopilot now refines what it used to accept;
  - a repeated crash stops within the cycle budget;
  - a crashed fan-out member doesn't count toward the quorum;
  - the reason reaches the campaign result.

  The tests that target the bug fail on `main`.
- **Regression:** 24 of 25 related test files give identical results to `main` (the 25th only adds a test) (refinement loop, health gate, dry-run gate, pipeline integration, convergence, local and cluster executors, cancel, fan-out and resume, MLIP loop, QC pipeline).

## Not covered (noted in #802)

- **Convergence ladder:** the ladder still reads crashed members' partial output.
- **`ClusterExecutor._finalize`:** a walltime-killed job's sacct exit code can parse as 0.
- **Caller-supplied `cancel_check`:** a cancel from such a hook returns `status="error"`. Under Autopilot, with the critic's fixes, that can submit one more job before stall detection stops it.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1766dbd228e5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.