# Issue

**Title:** report mode GPU is in and write out gpu UUID

## Description

## Description
Improve GPU logging information, inform user of potential issue with GPU compute mode, provide a better error message when `openmm.OpenMMException: No compatible CUDA device is available` is thrown.

 
 

## Todos
- [ ] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Changelog message

```
 #697 Improve GPU logging information, inform user of potential issue with GPU compute mode, provide a better error message when `openmm.OpenMMException: No compatible CUDA device is available` is thrown. Note, formatting of "CUDA devices available:" message has changed. 
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3cf98eb4e22a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.