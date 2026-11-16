# Issue

**Title:** Feature non linear time averaged msd

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- **msd.py** script was modified to support time-averaged mean square displacement calculation for dump files with non-linear time spacings.

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced? 
 Yes Issue #5028 
 - [x] Tests updated/added? 
 Not Yet
 - [x] Documentation updated/added?
 Updated short documentation at the top of the msd.py as comments
 - [x] `package/CHANGELOG` file updated?
 Not Yet
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!) 
 Yes - Added

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5066.org.readthedocs.build/en/5066/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91146c581eee`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.