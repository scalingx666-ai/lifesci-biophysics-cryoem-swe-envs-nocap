# Issue

**Title:** Returning and broadcasting _replica_thermodynamic_states

## Description

## Description
 by ensuring that `_mix_replicas` returns the `_replica_thermodynamic_states`. Also broadcasting it to the MPI context, according to [mpiplus broadcasting](https://github.com/choderalab/mpiplus/blob/216f1328d40f71e6554d909d470b6fe3104fde68/mpiplus/mpiplus.py#L226)

## Todos
- [ ] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91a4b98f20d6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.