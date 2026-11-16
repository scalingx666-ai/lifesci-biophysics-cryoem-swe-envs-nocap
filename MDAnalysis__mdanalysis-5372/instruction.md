# Issue

**Title:** ENH: support GMX linear angle potential

## Description

* Fixes gh-5361.

* Add support for parsing linear angle potentials in GMX TPR files. As described in the GMX docs linked in the matching issue, these are useful for planar molecules like CO2.

* TPR position and angle data-related regression tests have been added. I did check that the angle data test fails before the patch applied here.

* I believe there may be a serious unit/scaling issue in the TPR coordinate parser, but I'll open a separate ticket about that since it is not directly related.

## LLM / AI generated code disclosure
No AI usage here.

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added?
 - [ ] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5372.org.readthedocs.build/en/5372/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `523c853393cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.