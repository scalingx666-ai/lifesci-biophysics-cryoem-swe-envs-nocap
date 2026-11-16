# Issue

**Title:** backwards deserialization compatibility

## Description

## Description
This catches a `KeyError` when deserializing which prevents raising an error when deserializing objects from previous versions (0.21.4 or lower). It does so by showing a warning to the user.

 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
Backwards compatibility when deserializing netcdf (`.nc`) files from previous versions (<=0.21.4).
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a408bed85531`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.