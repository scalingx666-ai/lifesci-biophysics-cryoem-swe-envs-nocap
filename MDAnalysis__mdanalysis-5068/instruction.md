# Issue

**Title:** Update ARRAY numpy macros in transformations.c

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->

Possible investigation fix to #5061

Changes the old NPY_XX macro sto NPY_ARRAY_XXX macros

Got the hint here: 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Change instances of NPY_IN_ARRAY to NPY_ARRAY_IN_ARRAY in `transformations.c`
- Change instances of NPY_ALIGNED to NPY_ARRAY_ALIGNED in `transformations.c`
- Change instances of NPY_ENSURECOPY to NPY_ARRAY_ENSURECOPY in `transformations.c`


## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [X] Issue raised/referenced?
 - [ ] `package/CHANGELOG` file updated?

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0dc5a1bb5b4f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.