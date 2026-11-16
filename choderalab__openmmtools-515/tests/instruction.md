# Issue

**Title:** Remove logic for missing file when retrying to open a dataset

## Description

## Description
Provide a brief description of the PR's purpose here.

What should we do about these now defunct arguments to `_open_dataset_robustly`
`catch_io_error=False` and `io_error_warning=None`? Since the method is private, we should be able to just remove them from the function signature and update the code where we use it, thoughts?

## Todos
- [ ] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `347379b6310b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.