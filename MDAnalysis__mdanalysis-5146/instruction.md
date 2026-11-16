# Issue

**Title:** BUG: GMX tpr interm >= 103

## Description

* Fixes gh-5145.

* Support for intermolecular bonded interactions (`interm` variable in our `.tpr` parsing code) was not added until GROMACS tpx 103 (release 5.1: https://manual.gromacs.org/5.1/ReleaseNotes/new-features.html#added-support-for-inter-molecular-bonded-interactions)

* So guard against reading that value in accordingly, and add a regression test for properly parsing one of the problematic `.tpr` files shared in the matching ticket.


## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5146.org.readthedocs.build/en/5146/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e14ff82e204b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.