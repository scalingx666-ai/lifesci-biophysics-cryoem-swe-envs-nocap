# Issue

**Title:** changed btype, to btype as its causing error while running tests

## Description

As this is a small change i have not raised any issue.

Changes made in this Pull Request:

* Fixed the parameter name in `pytest.mark.parametrize` for `test_VE` in `testsuite/MDAnalysisTests/core/test_atomgroup.py`.
* Removed the unintended trailing comma from `"btype,"`, changing it to `"btype"` so that the parametrized argument correctly matches the test function signature.

## LLM / AI generated code disclosure

LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: No

## PR Checklist

* [ ] Issue raised/referenced?
* [x] Tests updated/added?
* [ ] Documentation updated/added?
* [ ] `package/CHANGELOG` file updated?
* [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
* [ ] LLM/AI disclosure was updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**[Developer Certificate of Origin](https://developercertificate.org/)**](https://developercertificate.org/), under the MDAnalysis [[LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE)](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cec335fa0ce5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.