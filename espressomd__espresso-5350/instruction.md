# Issue

**Title:** Fix recalc_forces flag in integrator

## Description

Integrator::integrate derived `reuse_forces` solely from `reuse_forces_flag`,
mapping `recalc_forces=True` to INTEG_REUSE_FORCES_CONDITIONALLY (0) instead of
INTEG_REUSE_FORCES_NEVER (-1). The `recalc_forces_flag` was read only for the
mutual-exclusion check and then dropped. As a result, after a prior run() left
propagation.recalc_forces == false, a subsequent run(0, recalc_forces=True)
silently skipped the force recompute (integrate.cpp recomputes only when
reuse_forces == NEVER or propagation.recalc_forces is already true), leaving
stale forces. This is observable e.g. with a Langevin thermostat, where the
re-drawn stochastic force should differ after the Philox counter advanced.

Fix: derive reuse_forces with three cases, restoring the pre-rewrite semantics
where recalc_forces=True maps to INTEG_REUSE_FORCES_NEVER. The preceding
mutual-exclusion check guarantees at most one flag is set, so the nested
ternary is order-independent.

Adds testsuite/python/integrator_recalc_forces.py (registered MAX_NUM_PROC 1),
which asserts that run(0, recalc_forces=True) after a Langevin run(1) changes
the forces.



🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `643abd2e698e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.