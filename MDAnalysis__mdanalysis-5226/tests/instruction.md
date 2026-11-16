# Issue

**Title:** Improve performance of wrap method

## Description

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Changed the wrap method to use index mapping instead of a loop.

The method was significantly slower than expected when wrapping compounds (residues, fragments, molecules, segments).  See the corresponding  on the MAICoS repo.

It just never got the same performance increases as for example `unwrap`. I considered using the `_split_by_compound_indices` approach from `unwrap`, but that is actually not needed for wrapping. Simply calculating the shifts and applying them in one go yields an enormous speed up. Most the performance penalty actually came from the `np.where` done in each loop I believe.

Here is a scaling plot comparing the speeds:

<img width="857" height="552" alt="output" src="https://github.com/user-attachments/assets/29c3a6aa-87f4-4c68-af4f-410e4aa3378b" />


## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: no

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
 - [ ] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `beaece1be7a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.