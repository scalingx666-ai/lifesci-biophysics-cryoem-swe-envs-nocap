# Issue

**Title:** sim analysis: 600 s default and timeout escalation

## Description

.

Simulation-analysis scripts ran under a 120 s limit with no escalation. That covers `analyze_output` post-processing and `check_observable_convergence` (#680), e.g. a Green–Kubo viscosity or an MSD diffusion coefficient. A long LAMMPS trajectory then either failed or was "fixed" by regenerating code that was never wrong. The experimental agents already get 600 s plus the shared escalation policy.

## Changes

- **Default timeout:** `BaseAnalysisAgent` (sim) now defaults `executor_timeout` to `executors.DEFAULT_TIMEOUT` (600 s), the same as the experimental agents. It was 120 s, set in January and carried into the codegen engine.
- **Escalation:** `_execute_script` runs under `_locked_exec.escalate_timeouts`, the shared policy from #699. A timed-out script is re-run **unchanged** at 2× the limit (600 → 1200 → 1800 s cap) before the refine loop sees the timeout. Any other error goes straight to the refine loop, as before.
- **Bounded:** a per-property `analysis_time_budget_s` (default 3600 s) bounds the timeout re-runs (`remaining_s`; the first limit is never clamped, so a started script finishes). It is also the QC engine's run deadline (`_run_deadline`: no further verification once spent) and its verification-loop budget (`qc_time_budget_s`). Without this bound, one property could block for hours (3 attempts × 3600 s plus refits), against the CLAUDE.md rule that the retries are bounded by the run's deadline and the loop's budget.
- **No re-climbing:** a rewrite after a timed-out script starts from the limit already reached, not from 600 s again.
- **Structured timeout signal:** `ScriptExecutor` now marks its own timeout with `"timed_out": True`, and the sim path keys on that flag. Matching `"timed out"` in the message would also catch a crash whose stderr says it (`socket.timeout`, a file lock), and re-run broken code unchanged.
- The sim base already imports the QC engine from `exp_agents`, so the policy is reused in place rather than moved.

Trajectory-size scaling (option 3 in the issue) isn't needed once timeouts escalate.

## Testing

New tests in `tests/test_sim_base_analysis_agent.py`:
- the default matches `DEFAULT_TIMEOUT`;
- a **real** script that needs ~2.5 s under a 2 s limit succeeds on the 4 s re-run (no refine pass, no LLM call; 1.5 s headroom);
- with a fake executor: the 600 → 1200 → 1800 schedule, the hand-off after the cap, the reached-limit floor for the next rewrite, and that a crash which merely *says* "timed out" is **not** re-run;
- re-runs never pass the property deadline (first limit unclamped);
- `compute_property` stamps the run deadline for the QC engine.

The four related suites: 74 passed. Across every test touching `ScriptExecutor`, the only failures are 2 in `test_run_tracked.py`, identical on `main`.

## Follow-ups (not in this PR)

- A re-run after a kill shares `OUTPUT_DIR` with the killed attempt, so a half-written cache or artifact could be picked up. The experimental path has the same exposure; it's worth isolating attempts there and here together.
- Move `escalate_timeouts` / `is_timeout_error` to `scilink/executors.py` so neither side depends on the other's private module.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e62ab5617c00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.