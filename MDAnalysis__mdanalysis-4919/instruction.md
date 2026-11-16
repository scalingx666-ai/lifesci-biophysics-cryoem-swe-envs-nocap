# Issue

**Title:** ENH: tpx 137 (GROMACS 2025.0) support

## Description

* Add support for GROMACS `2025.0` `.tpr` version 137, which mostly involves accounting for the new PyTorch neural network potential field, `F_ENNPOT`. Most of the other changes are similar to the approach I previously used for bumping `.tpr` version support in gh-4866.

* Some other `.tpr` additions are noted at https://github.com/MDAnalysis/mdanalysis/discussions/4888, but we don't appear to need to use those within the context of our current `.tpr` parsing tests. It may be sensible to occasionally generate `.tpr` test files that use those extra bells and whistles to see how we fare, though as a first pass here the full testsuite passes for us if we simply do the usual `gmx convert-tpr` at the latest stable GROMACS release tag `v2025.0` and propagate our current TPR testing to additionally use the converted files.


## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
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
📚 Documentation preview 📚: https://mdanalysis--4919.org.readthedocs.build/en/4919/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `596cd7cae905`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.