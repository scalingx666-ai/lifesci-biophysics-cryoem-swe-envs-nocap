# Issue

**Title:** Add gentle equilibration protocol as utility function

## Description

## Description
 

These changes add the function `gentle_equilibration` that can be imported using `from openmmtools.utils import gentle_equilibration`. 

These changes create a new utils module that should serve as a more organized API point where to locate all the utility functions.

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Changelog message
```
Support for gentle equilibration. API point in `openmmtools.utils.gentle_equilibration`.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aaa4719a7f00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.