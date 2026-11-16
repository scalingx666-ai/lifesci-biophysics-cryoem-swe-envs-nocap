# Issue

**Title:** Overriding deepcopying behavior to avoid creating new ContextCaches.

## Description

## Description
When you specify a context_cache to be used in the mcmc_move for the simulation it gets deep copied into every replica. This avoid creating new ContextCache for each replica, instead it uses a reference to the same given and hopefully compatible ContextCache.

 

## Todos
- [x] Implement feature / fix bug
- [x] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.
- [ ] Check how this affects serialization of objects.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a9c61cea5c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.