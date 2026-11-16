# Issue

**Title:** Remove HREX replica-state distribution convergence plot

## Description

* Removes `plot_hrex_replica_state_distribution_convergence` from `timemachine.fe.plots`. This plot has never been particularly useful, and the current implementation uses a lot of memory (~300 MB according to local profiling).
* Miscellaneous organization/cleanup
  - moves `HREXPlots` to `timemachine.fe.free_energy` alongside `PairBarPlots` (there's no plotting code in `timemachine.md.hrex`, and the abstractions in the latter are higher-level than the plots, e.g. not assuming a 1-d chain of states)
  - removes an unused argument from `plot_chiral_restraint_energies`

**NOTE**: merging this will require downstream changes

Example of the removed plot:
![image](https://github.com/proteneer/timemachine/assets/319411/1d6b8067-ead3-4288-b5c2-e90dacd154f5)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4136a4e6471`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.