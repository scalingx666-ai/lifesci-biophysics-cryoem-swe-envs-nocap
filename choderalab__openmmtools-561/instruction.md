# Issue

**Title:** Real time analysis information file

## Description

## Description
The purpose of this PR is to have a way to export a human readable file with real time analysis information.

CC 

The main idea is taking the information straight from the reporter to avoid redundancy and synchronizing problems.

So far I've settled into using YAML for this file but I am still open for suggestions. With a yaml file being easy to append/update without having to read the whole file contents (as opposed to JSON). 

## Todos
- [ ] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8bf446742f75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.