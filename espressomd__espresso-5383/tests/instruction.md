# Issue

**Title:** Add EK species/container tau cross-check

## Description

EKContainer::add_in_core validated only floating-point precision and the
MPI Cartesian-communicator observer, never comparing the new species' tau
against the container's tau. An EKSpecies derives all of its MD<->lattice
conversion factors (diffusion, flux, energy, external e-field) from its own
tau, while the integrator and the MD-tau veto use the container tau. When
the two differ, effective diffusion/flux/mobility are wrong by a factor of
tau_species/tau_container with no error raised: ekcontainer.add(species)
succeeded silently.

Add a tau comparison inside the existing parallel_try_catch lambda, next to
the precision/observer checks, throwing std::runtime_error when the species
tau differs from the container tau. The check runs under the same
coordinated parallel_try_catch on every rank, so it cannot diverge. This
PREVENTs the inconsistent input rather than silently rescaling baked-in
conversion factors, matching how every other add-time mismatch in this path
is treated and the documented contract that tau must be an integer multiple
of the MD time step on both classes.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7c9487eda65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.