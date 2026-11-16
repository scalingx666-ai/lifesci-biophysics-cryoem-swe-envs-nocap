# Issue

**Title:** Allow using selection keywords as residue names

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->
 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- selection keywords can now be used as residues names. The keyword must be escaped, e.g. `u.select_atoms("resname \\water")`
- add tests for selecting residues named 'protein'. Add test to check `u.select_atoms("resname protein")` still raises a `SelectionError`
- update documentation on selection keywords to show how to escape selection keywords

Note, this does mean that any keyword can be escaped - not just 'protein', 'water', etc. But it seemed like the simplest solution.

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [X] Issue raised/referenced?
 - [X] Tests updated/added?
 - [X] Documentation updated/added?
 - [X] `package/CHANGELOG` file updated?
 - [X] Is your name in `package/AUTHORS`? (If it is not, add it!)

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5119.org.readthedocs.build/en/5119/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0685ac92b7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.