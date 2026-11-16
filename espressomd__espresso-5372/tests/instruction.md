# Issue

**Title:** analysis: raise instead of NaN for empty particle types

## Description

check_particle_type only verifies 0 <= p_type <= max_seen_particle_type,
and max_seen is never decremented when particles are removed. A
seen-then-deleted type (or any populated-then-emptied type) therefore
passes validation while contributing an empty buffer / zero total mass.

On such a type the analysis routines divided by zero:
- center_of_mass returned com/mass = 0/0 = NaN (the `double mass = 1.`
  placeholder was a red herring: boost::mpi::reduce overwrites the root's
  output with the reduced sum, so it never prevented the division).
- gyration_tensor divided by buf_pos.size() == 0; its non-empty
  precondition was only a release-stripped assert(), so it produced NaN
  in Release and aborted in RelWithAssert.
moment_of_inertia_matrix inherits center_of_mass, so it threw too.

Fix (PREVENT, matching the subsystem convention where bad analysis
preconditions throw):
- center_of_mass: all_reduce the total mass to every rank and throw a
  std::runtime_error when it is zero. The throw is collective so the
  script interface does not deadlock on the following MPI collective.
- gyration_tensor: replace the assert with a real throw on rank 0.
- Analysis.cpp: move the core calls and their MPI collectives inside
  context()->parallel_try_catch (mirroring calc_rg) so the new throws
  are marshalled correctly across MPI.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d96515884cbe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.