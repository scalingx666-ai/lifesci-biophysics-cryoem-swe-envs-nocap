# Issue

**Title:** `versionchanged` adjustment for `LAMMPS.DumpReader`

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 

As mentioned in the Issue the docs state that in [LAMMPS.DumpReader](https://docs.mdanalysis.org/2.9.0/documentation_pages/coordinates/LAMMPS.html#MDAnalysis.coordinates.LAMMPS.DumpReader) the arbitrary columns were supposedly implemented in 2.7.0 but the according to the CHANGELOG it was in version 2.8.0

Additionally, The # was for the PR #3608 and not the Issue #3504. To make it consistent with the other case where an Issue is mentioned in the docs I adjusted it to refere to the Issue# and not the PR#

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Adjustment of `versionchanged` to `2.8.0` in accordance to the CHANGELOG
- Adjustment of the reference to the Issue to maintain consistency in Docs

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [ ] Tests updated/added?
 - [x] Documentation updated/added?
 - [ ] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5091.org.readthedocs.build/en/5091/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b4ab7e5f2223`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.