# Issue

**Title:** Fix failing CI tests

## Description

## Description
This PR fixes failing tests by
* Changing the behavior of `openmmtools.utils.available_platforms()` to filter by minimum available precision, which defaults to `mixed`
* Changing the behavior of `openmmtools.utils.temporary_directory()` to issue a warning (instead of an Exception) if the temporary directory fails at being cleaned up

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `23e81844338b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.