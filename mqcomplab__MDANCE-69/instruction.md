# Issue

**Title:** ci: set up and verify CI workflow for automated testing

## Description

Motivation: to have CI for the MDANCE.

Features:
- Configured GitHub Actions to run tests on multiple Python versions (3.9 and 3.11).
- Integrated caching to speed up dependency installation.
- Ensured pytest runs successfully and reports test results.
- Verified package installation and import in the CI environment.

Fix:
```
sorted_comps = np.argsort(-comp_sims)
```
This line in `diversity_selection` with `strat` option has been changed from `argsort()` (default version using quicksort) to `argsort(kind='stable')` (stable quicksort). There is no significant impact. The problem only arises for ties and it will differ between CPUs. The new version maintains the relative order of records in the case of equality of keys.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `09a62f6ee488`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.