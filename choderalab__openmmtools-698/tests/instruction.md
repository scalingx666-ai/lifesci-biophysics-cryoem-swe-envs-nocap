# Issue

**Title:** Importing everything in utils module

## Description

## Description

Importing all members of the `utils.py` submodule.

In previous changes we created a `openmm.utils` package/module with a `utils.py`sub module for the sake of organizing and modularizing many of the utility functions we have in openmmtools. By doing so I mistakenly only imported some of the members of the modules instead of importing all of them.

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) to summarize changes in behavior, enhancements, and bugfixes implemented in this PR

## Status
- [ ] Ready to go

## Changelog message
```
We are now importing all the members of the new `utils.py` submodule by default. Recovers previous behavior of importing utils with `from openmm.utils import ...`, avoiding API breaking changes.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c491c3d205db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.