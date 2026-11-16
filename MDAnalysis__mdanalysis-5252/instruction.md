# Issue

**Title:** Optimize inverse index mapping in groups.py (Fixes Issue #3387)

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->


## Summary

This PR replaces the O(n²) inverse index reconstruction logic in groups.py with an O(n) implementation written in Cython.

The previous implementation used np.where inside a loop over unique indices, leading to quadratic complexity. The approach in this PR uses a dictionary-based lookup implemented in lib/_cutil.pyx, where the indices of the unique values in `indices` array are added to a map. Then `self.ix` array is iterated over only once and the map is consulted to get the index from the uniqe value array. 
This means that the `self.ix` array is traversed in a single pass, making it linear time complexity.

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Implemented a new cython function inverse_int_index() in package/MDAnalysis/lib/_cutil.pyx
- Replaced the previous Python O(n²) logic in groups.py with a call to the new cython function
- Benchmarked performance improvement

## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## Benchmark
For an array of size 1,000,000 with ~5000 unique values, following speedup was observed :
Python: ~0.41s
Cython: ~0.11s
Speedup: ~3.64 times
(these values are from local testing)

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5252.org.readthedocs.build/en/5252/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `16ac6bae5afe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.