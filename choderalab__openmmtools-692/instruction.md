# Issue

**Title:** Appending real time stats

## Description

## Description
Changing the default behavior of the multistate reporter to append in the YAML real time statistics file instead of overwriting.

We already had the machinery for this, we probably want to discuss if we want to expose this option to the user.

I tested this resuming a run, but we probably want to write a proper test for this behavior. WIP.

 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [x] Ready to go

## Changelog message
```
Real time statistics in YAML file are now appended when resuming simulations, instead of being overwritten.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `abb2f615b730`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.