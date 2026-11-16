# Issue

**Title:** fix links to LAMMPS docs

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
Fixes #

Changes made in this Pull Request:
- links broke when LAMMPS moved from lammps.sandia.gov to lammps.org
- `git grep -l sandia | xargs perl -wpi~ -e 's(http://lammps\.sandia\.gov/doc)(https://docs.lammps.org)' --`
- `git grep -l sandia | xargs perl -wpi~ -e 's(http://lammps\.sandia\.gov)(https://www.lammps.org)' --`

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [ ] Issue raised/referenced?
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
📚 Documentation preview 📚: https://mdanalysis--5063.org.readthedocs.build/en/5063/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e604e4a51f9b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.