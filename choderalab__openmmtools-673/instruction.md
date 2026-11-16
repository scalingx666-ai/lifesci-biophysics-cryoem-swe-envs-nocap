# Issue

**Title:** Timing information at INFO logging level

## Description

## Description
This changes the logging level of the iteration timing information to INFO. Since it is not really debug information but actually
information that users do want to get by default.

 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
Users are expected to get the timing information of each iteration by default. This information is now at the `INFO` logging level, instead of `DEBUG`.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `139b4540fb8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.