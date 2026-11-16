# Issue

**Title:** tests: modernize deprecated numpy assertions in test_gnm.py

## Description

Contributes to issue #3743.

This PR updates deprecated NumPy testing functions in test_gnm.py:
- Replaces assert_almost_equal and assert_array_almost_equal with numpy.testing.assert_allclose
- Uses pytest.approx for scalar floating point comparisons
- Preserves existing tolerances and test behavior
- All tests pass locally


Added myself to package/AUTHORS as a first-time contributor.




<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5178.org.readthedocs.build/en/5178/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `379d2960dc75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.