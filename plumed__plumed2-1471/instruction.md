# Issue

**Title:** Add synchronized shared-path deposition weights to OPES

## Description

Add opt-in `WALKERS_SHARED_BIAS` for synchronized replicas of one complete path. Each deposition uses the mean of the bead-local bias plus `EXTRA_BIAS`, divided by kBT, as a common log weight. Use this only when the sampled bias Hamiltonian is the same bead average. This changes deposition weights, not forces.

The option requires `WALKERS_MPI`, explicit `SIGMA`, equal temperature and `PACE`, consistent flags, and no `EXCLUDED_REGION`; it is unavailable in `OPES_METAD_EXPLORE`. Histories record the mode and bead count and reject incompatible restarts. Ordinary walkers keep their local weights. Kernel/walker counts do not represent independent complete-path samples.

Updated against current master, retaining the merged OPES kernel, neighbor-list and restart changes. The effective diff is limited to `OPESmetad.cpp` and its native shared-path regression. The earlier prerequisite fixes are already upstream; this PR no longer carries separate infrastructure fixes.

Validation: release and debug/glibcxx-debug MPI builds pass `make nmcheck` and all 47 native OPES regression suites with `PLUMED_ALL_TESTS=yes`. The shared-path test checks independently calculated log weights, EXTRA_BIAS, finite-difference forces, restart contracts and collective rejection, plus full-sum/neighbor-list parity in both local/shared and bare/wall modes. No numerical reference tolerances were relaxed.

The native manual generator and MkDocs build completed. The OPES_METAD page has no failing examples or undocumented keywords. The full manual still reports 36 other pages with failing examples and 27 MkDocs warning lines; a warning-free full manual and the full optional-dependency suite are not claimed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `626122668a1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.