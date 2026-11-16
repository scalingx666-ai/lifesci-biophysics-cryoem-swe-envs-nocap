# Issue

**Title:** Modernize assertions in test_distances.py (part of #3743)

## Description

Contributes to #3743

### Changes made in this Pull Request:
- Modernized assertions in `testsuite/MDAnalysisTests/analysis/test_distances.py`
- Replaced deprecated `assert_almost_equal` / `assert_array_almost_equal` with `numpy.testing.assert_allclose` and `assert_array_equal` where appropriate
- Replaced old float comparison patterns with modern NumPy/PyTest assertions
- Ensured no behavioural changes to distance calculations

## PR Checklist
- [x] Issue raised/referenced? (Contributes to #3743)
- [x] Tests updated/added? (Updated test assertions)
- [ ] Documentation updated/added?
- [ ] `package/CHANGELOG` file updated?
- [x] Is your name in `package/AUTHORS`?

## Developers Certificate of Origin
I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5158.org.readthedocs.build/en/5158/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5c11b509fa65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.