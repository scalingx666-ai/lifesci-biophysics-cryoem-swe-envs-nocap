# Issue

**Title:** FIX: Use mean aggregation for InterRDF bins

## Description

FIX: update bin calculation to use ndarray_mean in aggregator of InterRDF and correct InterRDF tests to correctly support multiple frames


Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- **Fixed parallel aggregation for bins:** Updated the `ResultsGroup` aggregator in `InterRDF` to use `ndarray_mean` instead of `ndarray_sum` for `bins`.
- **Added multi-frame support to tests:** Modified the `sels` fixture in `test_rdf.py` to generate a mock trajectory with multiple frames.
- **Adjusted `test_double_run`:** Removed the parallel backend parametrization for `test_double_run`, restricting it to the serial backend.
- **Renamed duplicated `test_norm`**

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
 - [X] Tests updated/added?
 - [X] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [X] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [X] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5344.org.readthedocs.build/en/5344/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1729aadff1d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.