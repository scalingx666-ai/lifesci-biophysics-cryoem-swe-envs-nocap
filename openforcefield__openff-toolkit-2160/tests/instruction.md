# Issue

**Title:** Improve `Molecule.remap` docstring

## Description

When working with `Molecule.remap` today, I came across a few behaviors which weren't documented and I had to discover through trial-and-error.

* This method returns a new object; I thought it modified in-place
* Conformers are updated - I don't need to shuffle them myself
* Same with partial charges
* This API doesn't seem particularly experimental, or at least not more unstable than I'd expect from a 0.x package (#2082)

I can move this into an issue but it's minor and, I hope, self-explanatory.

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [x] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://docs.openforcefield.org/projects/toolkit/en/stable/users/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c738b26a10e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.