# Issue

**Title:** Parallelize lindensity

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Parallelizes the mass and charge density profile calculation class (`MDAnalysis.analysis.lineardensity.LinearDensity`). As density profiles are computed independently for each timestep, the current parallelization methods allow the calculation of the density profiles without any problems.
- Some variable calculations (masses, charges) are moved to enable parallelization
- a boilerplate fixture to testsuite/analysis/conftest.py, analogous with existing ones
- a client_... fixtures to all tests using in testsuite/MDAnalysisTests/analysis/test_lineardensity.py, and modify the way run() method is called

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [x] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5007.org.readthedocs.build/en/5007/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e64755cb1999`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.