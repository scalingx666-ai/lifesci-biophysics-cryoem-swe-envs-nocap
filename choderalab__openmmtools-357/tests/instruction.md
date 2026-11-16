# Issue

**Title:** Remove commented out code

## Description

- [x] Implement feature / fix bug
 - [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
 - [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
 - [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)

1.  Cleans up some old commented out code.
2.  Regarding your comment (code "commented out") in #356: these variables are actually set upstream in the class hierarchy, which I've confirmed in actual usage.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eacae9b346ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.