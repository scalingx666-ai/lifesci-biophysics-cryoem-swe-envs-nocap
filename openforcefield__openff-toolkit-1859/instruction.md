# Issue

**Title:** Update NAGL pins in test environments

## Description

Would it be possible to update the NAGL pins to stay current with new 0.3.x releases? Just to check that newer releases don't break anything terrible here.
I dropped Python 3.9 in 0.3.8, whereas the toolkit still tests on it, so I've set the lower bound to 0.3.7 here.

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [ ] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53db31fdb3ff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.