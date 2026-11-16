# Issue

**Title:** Replacing Fire minimization with LocalEnergyMinimizer

## Description

## Description
Replaces `FireMinimizer` with `LocalEnergyMinimizer` to do the replica minimization.

This should enhance the stability of minimization. Supersedes #557 

 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
L-BFGS based `openmm.LocalEnergyMinimizer` is now used instead of `FireMinimizer` for energy minimization in replicas.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b7de2aace37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.