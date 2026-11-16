# Issue

**Title:** Fix Stokesian Dynamics MPI deadlock on missing per-type radius

## Description

Bug: StokesianDynamics::propagate_vel_pos evaluated radii.at(p.type) inside
the rank-0-only block, between the collective gather_buffer and the collective
scatter_buffer. For a particle whose type has no registered radius, at() threw
std::out_of_range on rank 0, which unwound out of the integration loop (no
parallel_try_catch on that call path) before reaching MPI_Scatterv. The other
ranks blocked forever in the matching MPI_Scatterv -> indefinite MPI deadlock
(multi-rank) or an uncoordinated std::out_of_range abort (serial).

Fix: replace radii.at() with radii.find() and, on a missing type, register a
coordinated runtime error via runtimeErrorMsg() instead of throwing. The rank-0
branch still falls through to the collective scatter_buffer (shipping zeroed
velocities), so every rank reaches MPI_Scatterv and no deadlock occurs. The
integration loop's collective check_runtime_errors(comm_cart) then turns the
registered message into a clean cross-rank ESPResSo runtime error
("Stokesian Dynamics: no radius defined for particle type N"). This mirrors the
existing SD precondition checks in integrate.cpp.

Test: testsuite/python/stokesian_missing_radius.py (NO_MPI) launches the
offending scenario as a child mpiexec -n 2 job under a 45 s timeout; on the
unfixed core it times out (deadlock), on the fixed core it exits cleanly with
the coordinated error.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0571bbb812b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.