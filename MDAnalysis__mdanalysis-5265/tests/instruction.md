# Issue

**Title:** Fix add_transformations broken state after invalid input

## Description

Changes made in this Pull Request:
- Added input validation in `add_transformations` to raise `TypeError` if any transformation is not callable, preventing broken trajectory state
- Added test `test_add_transformations_rejects_non_callable` in `testsuite/MDAnalysisTests/coordinates/base.py`

LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

- [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`?
 - [x] LLM/AI disclosure was updated.
 
I certify that I can submit this code contribution as described in the Developer Certificate of Origin, under the MDAnalysis LICENSE.



<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5265.org.readthedocs.build/en/5265/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `900d20b7e838`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.