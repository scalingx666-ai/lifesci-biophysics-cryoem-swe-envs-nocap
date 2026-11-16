# Issue

**Title:** Validate tune_skin parameters

## Description

The script-interface tune_skin handler forwarded tol and int_steps
straight into System::tune_verlet_skin with no bounds check, and neither
the auto-bound Python wrapper nor the core performed any validation. For
tol <= 0 the core bisection loop `while (fabs(a - b) > tol)` never
terminates (fabs(a - b) >= 0 is always > a negative tol, and once the
interval collapses the midpoint stops progressing), so tune_skin spun
forever with no exception. A non-positive int_steps additionally divides
by zero in the per-step timing.

Reject both invalid inputs in the SI handler, wrapped in
parallel_try_catch (matching the sibling node_grid/skin validation in
this file) so the error surfaces as a clean, MPI-coordinated Python
exception (std::domain_error) instead of a hang.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fddbe5320429`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.