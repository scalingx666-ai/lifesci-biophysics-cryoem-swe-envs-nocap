# Issue

**Title:** Fix critical bug in which GPU contexts would fail to recognize mixed precision was available

## Description

## Description
This PR , where GPU contexts would fail to recognize mixed precision was available.
This may mean that applications were silently failing to use mixed precision on GPUs.

I fixed the logic to match the function docstring, but I'm not sure if this will break anything else. We'll have to wait for the tests to see.
I've verified the new `test_platform_supports_precision()` test passes on an NVIDIA GTX 1080.

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [x] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [x] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3f2cfc0d9ea5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.