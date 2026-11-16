# Issue

**Title:** MAINT: Replace deprecated numpy assertions in coordinates/base.py

## Description

Contributes to #3743

Changes made in this Pull Request:
- Replaced deprecated `assert_almost_equal` and `assert_array_almost_equal` with `assert_allclose` in `testsuite/MDAnalysisTests/coordinates/base.py`.
- Converted decimal precision to `atol=10**(-decimal)` and `rtol=0` to maintain original test strictness.
- Added contributor name to `package/AUTHORS`.

## PR Checklist
- [x] Issue raised/referenced?
- [ ] Tests updated/added?
- [ ] Documentation updated/added?
- [ ] `package/CHANGELOG` file updated?
- [x] Is your name in `package/AUTHORS`?


<!-- readthedocs-preview mdanalysis start -->
----
 Documentation preview : https://mdanalysis--5156.org.readthedocs.build/en/5156/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21f7d80c4f78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.