# Issue

**Title:** Comply with Openmm 8 global default values

## Description

## Description
Testing of `RestorableOpenMMObject` depended on having `openmm.Force` objects with different default values for global parameters. Since, we have to change the tests accordingly.

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `988485e3456f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.