# Issue

**Title:** Add test for repex mixing with MPI

## Description

I'm opening the PR right now just to share the minimal test case that reproduces the error described in choderalab/yank#1130 (the code is [here](https://github.com/choderalab/openmmtools/blob/fix-mpi-mixing/openmmtools/tests/test_sampling.py#L1478-L1583)).

To run it with MPI, simply run `nosetests` with `mpiexec` as usual
```
build_mpirun_configfile "nosetests openmmtools/tests/test_sampling.py:TestReplicaExchange.test_uniform_mixing -s"
mpiexec.hydra -f hostfile -configfile configfile
```
I still have to figure out the reason, but at least this is a more manageable test case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `110524bc5079`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.