# Issue

**Title:** Add neighborhood support to MultiStateSampler and ReplicaExchange

## Description

As discussed, I'm working on first adding support for restricted neighborhoods to the existing samplers before implementing SAMS.

Presuming we keep the current order of mix-propagate-energy, my first hurdle is to determine how to store the neighborhood that was used to compute the replica energies at a subset of thermodynamic states.

@andrrizzi @Lnaden : I'd appreciate your advice here! Should I:
1. Extend [`read_energies` and `write_energies`](https://github.com/choderalab/yank/blob/sams-neighborhood/Yank/multistate/multistatereporter.py#L690-L778) to also read and write an `[n_iterations, n_replicas, n_states]` integer array that is 1 if the energy was computed for that state and 0 if not?
2. Modify the current energy read/write code to use [masked arrays](http://unidata.github.io/netcdf4-python/#netCDF4.Variable.set_auto_mask) and simply mask the values of the existing [`energies`](https://github.com/choderalab/yank/blob/sams-neighborhood/Yank/multistate/multistatereporter.py#L742-L747) matrix that are not computed?

I'm thinking option (2) might be simpler, though I've never used masked arrays before. Has anyone had experience with these?

### Progress

- [x] Add `_neighborhood(state_index)` method to compute local neighborhood
- [x] Restrict energy computation to neighborhood
- [x] Store energies only within neighborhood, alongside neighborhood information
- [x] Add test to ensure that energy array is properly stored when locality is restricted
- [x] Test restricted neighborhood `MultiStateSampler`
- [x] Implement fast online analysis
- [x] Implement locality support in replica exchange and parallel tempering
- [ ] Implement locally weighted histogram analysis for offline analysis: [paper1](http://www.stat.rutgers.edu/home/ztan/Publication/SAMS_preprint.pdf),  [paper2](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4729400/pdf/JCPSA6-000144-034107_1.pdf) - bumping this to a subsequent PR

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97de5cd24fd1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.