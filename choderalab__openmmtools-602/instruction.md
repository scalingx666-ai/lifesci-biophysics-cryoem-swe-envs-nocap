# Issue

**Title:** Returning velocities in sampler state when propagating replicas

## Description

## Description
Velocities were not being passed when returning the sampler state when propagating replicas.

 

Note that the bug is only present when MPI (multiple GPUs) are being used. We need to test our software using MPI as well (issue #603 )

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `47f59f71aefe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.