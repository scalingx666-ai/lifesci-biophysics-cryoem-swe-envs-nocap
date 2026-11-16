# Issue

**Title:** updating depreciated getargspec to getfullargspec that returns 7 values

## Description

This updates the depreciated `getargspec` function to `getfullargspec`. This returns 7 values, whereas `getargspec` returned only 4

ArgSpec(args, varargs, keywords, defaults)
FullArgSpec(args, varargs, varkw, defaults, kwonlyargs, kwonlydefaults, annotations)

 - [ ] Implement feature / fix bug
 - [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `79d8b7e71cab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.